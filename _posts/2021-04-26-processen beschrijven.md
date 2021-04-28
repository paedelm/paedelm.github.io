---
layout: post
title: "Beschrijven van processen om ze geautomatiseerd te draaien"
date: 2021-04-26
---

## Achtergrond

 In het kader van een betrouwbaar en veilig IT domein, is het idee om de processen in Git te beschrijven en met die beschrijving ze ergens anders geautomatiseerd en beheerst te laten draaien als zogenaamde **_services_**. Vanwege dat automatisme zal de beschrijving getypeerd moeten worden. Eenduidigheid is nodig, je hebt geen mogelijkheid meer om te vragen "wat bedoel je". Er zijn heel wat eisen te stellen aan de beschrijving, denk aan de mogelijkheid van uitbreiden van het schema zonder oude beschrijvingen te herschrijven, ondersteuning in een IDE zoals intellisense, maken van eigen templates voor bekende proces patronen, eenvoudige syntax en nog meer. 
 
 Mijn keuze is niet op een export beschrijving gevallen maar op een programmeertaal die bekende export formaten kan produceren. Een programmeertaal omdat daar alles inzit voor hergebruik, conditioneel genereren, indelen in sourcefiles, gebruik van intellisense, pre-processing zoals compileren, controleren zoals testen en nog veel meer. Uiteindelijk ben ik op F# terecht gekomen het heeft de voordelen van een dotnet omgeving met zijn uitgebreide library, het heeft een eenvoudige syntax, het heeft string interpolatie voor templates, condities zijn expressies en geen statements, en het allerbelangrijkste is de [Discriminated Union](https://fsharpforfunandprofit.com/posts/discriminated-unions/). C# kwam in de buurt maar heeft nog niet alles wat F# kan. Ik heb me beperkt tot DotNet en daarbinnen gericht op sterk getypeerde talen met mogelijkheid tot reflectie. 

## Wat moet je beschrijven?

 Een service bestaat uit een programma flow, de main Step, die periodiek wordt gestart om zijn werk te doen. Zo'n flow is een recursieve structuur: de flow bestaat uit 1 of meerdere stappen en een stap is een flow of een Task. Zie de definitie in F#:
 ~~~
// type Program is hier terwille van de eenvoud niet gespecificeerd
 type Task = {
    Id: TaskId;
    ShowLog: bool;
    Program: Program
    Params: string[]
}
type FlowType =
    | UntilError
    | UntilOk
    | Unconditional
    | Parallel
type Step = 
    | Task of Task
    | Flow of Flow 
and Flow = {
    Id: TaskId;
    ShowLog: bool;
    Type: FlowType
    Steps: list<Step>
 }

// een proces draait periodiek en kan op events reageren door de ...
// optionele hier terwille van de eenvoud niet gespecificeerde WatchParam

type TriggerConditionRec = {
    Interval: int;
    WatchParam: WatchParam option;
}
type Service = {
    Name: string;
    Program: Step;
    TriggerConditionRec: TriggerConditionRec
}

 ~~~
Je bent vrij een flow samen te stellen van van verschillende programma's zoals je dat ook zou doen als je er een shell script van maakt. Het flowtype "UntilError" geeft aan of je de flow wil stoppen bij de eerste stap die fout gaat, dit zal je zeker gebruiken als jouw service pas kan starten als er aan bepaalde voorwaarden is voldaan, bijvoorbeeld bij de aanwezigheid van een bestand, is het niet aanwezig of nog niet oud genoeg dan wacht de service tot de volgende periode. Met de watch parameter kan je de wacht periode bekorten om sneller te reageren op bijvoorbeeld de aanwezigheid van een bestand. Er zijn meerdere flowtypes.

Hieronder staat de definitie van Program:
~~~
// Terwille van de eenvoud is niet alles weergegeven
type ExternScript = {
    Runner: string;
    ContentZipOfProgDir: ContentZipOfProgDir option;
    StdinRedirect: Redirect option;
    StdoutRedirect: Redirect option;
}
type ExternProgram = {
    Id: string;
    Env: Dtap;
}
type InputFileSelect = {
    FileSelectExpression: FileSelectExpression;
    FileAgeInMilliSeconds: int;
}
type FileProgram = {
    Dir: string;
    InputFileSelect: InputFileSelect;
    OutputParameter: OutputParameter option;
    Program: Program;
}
and Program =
    | ExternScript of ExternScript
    | ExternProgram of ExternProgram
    | FileProgram of FileProgram

~~~
De essentie hiervan is dat type "ExternScript" als child process wordt opgestart, met de mogelijkheid tot redirect van stdin en stdout en ook de mogelijkheid om een eigen directory tree door te geven. Type "ExternProgram" is een DLL aanroep dus kent het ook geen redirects, deze mogelijkheid is gemaakt voor snelle checks en enigzins gevaarlijk omdat het niet in een child process draait.

Type "FileProgram" is een koppeling van bestanden aan een "Program", eigenlijk een _pseudo_ "Program", met property "InputFileSelect" geef je aan dat voor ieder bestand dat voldoet aan die conditie, het programma wordt opgestart. Hiermee kan je binnen de service toch bedoeld parallel verwerken.

Hieronder volgt een definitie voorbeeld:
~~~
let pythonScript env  =  $"""
{DemoScriptV1.envScript env}
{System.IO.File.ReadAllText "services/demoscript/demoscript.v2.py"}
"""

let Service name env = 
    let program = Task {
            Id = sprintf "%s.%A.%d.%d" name env version revision
            ShowLog = true;
            Program  = ExternScript {Runner = "python"; ContentZipOfProgDir = None; StdinRedirect = None; StdoutRedirect = None }
            Params = [|"-c"; (pythonScript env).Replace("\r\n","\n"); name|]
        }
    match env with
    | DEVL | TEST -> Some {
        Name = name;
        Program = program;
        TriggerConditionRec = DemoScriptV1.TriggerConditionRec
        }
    | _ -> None
~~~
Ter verduidelijking: De source bedient alle mogelijke omgevingen het bevat een functie die als parameter de omgeving mee krijgt en die als resultaat het item voor de bewuste omgeving terug geeft.  Als het source systeem gaat genereren, dat doet het als het runt, worden jouw functies aangeroepen met de juiste versie voor de juiste omgeving, en dat is gebaseerd op het _change_ systeem waar iedere versie van een service is gekoppeld aan een _change_. En de levensloop van de _change_ van Development tot Productie loopt. Als de _change_ wisselt van omgeving moet er opnieuw gegenereerd worden. Je hebt dus alle mogelijkheden om aanpassingen te doen aan de inhoud van alles wat zich in de Service bevindt. Daarnaast kan je aangeven of je wel een service hebt voor die omgeving. Dat heet een option, je geeft terug "Some Value" of "None"

 Als eerste wordt hier het pythonScript aangemaakt. Het bestaat uit environment variabelen en een python source die ingelezen wordt vanuit een file. Er wordt van het zo ontstane python script een Task aangemaakt met een Program van het type ExternScript met als runner python waaraan ons script doorgegeven. Dit Program wordt gebruikt in ons Process en dat wordt alleen gegenereerd als we in DEVL of TEST omgeving zitten. In de overige omgevingen wordt None geproduceerd.

## Waar is rekening mee gehouden bij het kiezen van het beschrijvings systeem?

* De beschrijving moet na import gelijk zijn aan wat het voor export was.
  
  De beschrijving van het domein wordt in zijn geheel geëxporteerd en ter controle geïmporteerd, ze moeten dan gelijk zijn. Wat het export formaat is doet niet ter zake maar in de praktijk zal het versleutelde json zijn. Een eis is dat het onleesbaar is zonder sleutel.

* Je moet er alles in kwijt kunnen.
  
  Voorlopig kan alles er in behalve functions. Waar het source systeem met functions meerdere omgevingen bediend, ligt bij het export formaat de omgeving vast. Het is dus een export voor die bewuste omgeving.Omdat je byte arrays kan exporteren is al het exotische mogelijk. Ik heb daar al op voor gesorteerd door een directorytree(_ContentZipOfProgDir_) als optie op te nemen bij een _ExternScript_. Als voorbeeld hiervoor gebruik ik een java programma die voor exporteren wordt gecompileerd en waarvan de gecompileerde jar-file via de _ContentZipOfProgDir_ property mee komt in de export. 

* Optimaal gebruik maken van de Ide.

  Ik gebruik VisualStudio Code, bij de code in F# voor het opbouwen van de beschrijving middels het Type systeem kan je intellisense gebruiken en wijst de Ide je op je fouten. Maar ook het python script, java programma of welke source dan ook, de Ide herkent het meestal wel of er is wel een plugin voor te krijgen en je hebt ondersteuning van de Ide bij het schrijven daarvan.

  Soms moet je kiezen: de source van een script in een string constante met interpolatie of in een file met de juiste extensie. Gelukkig kan je beiden in F#, die aparte file geeft als voordeel dat de Ide je ondersteunt maar met de string constante (met interpolatie) kan je makkelijker templates programmeren.

* Testen en controleren

  Door te kiezen voor een actief systeem, jouw code wordt aangeroepen om iets te exporteren, is het ook makkelijk om controles in te bouwen, zeg maar unit tests. De eind controle probeer ik af te dwingen door jouw te vragen voor iedere versie van de service een script te leveren om test gegevens klaar te zetten, dat _test-initialisatie-script_ wordt gecombineerd in een UntilError flow met de service flow. Het resultaat is een eenmalige test van jouw service, als die fouten geeft, stopt de export.

* Meerdere versies van services bijhouden en die koppelen aan de juiste omgeving.
  
  Je hebt meerdere versies nodig want het starten van de ontwikkeling van een _change_ gebeurt niet direct in de productie omgeving, je mag pas getest in productie, dat betekent dat je in zo'n geval minimaal 2 versies onderhoudt.  

  Op basis van een afgedwongen koppeling (door de compiler) tussen een versie van de service en een beschrijving van een _change_, zorgt het promoten van de _change_ naar een andere omgeving, bijvoorbeeld van **_Acceptatie_** naar **_Productie_** dat de versies van de services die daaraan gekoppeld zijn gebruikt worden voor de **_Productie_** export.

* Afdwingen van correctheid gebeurt door het typeren van de gegevens.  
 
  De compiler controleert jouw source systeem op de spelregels en de typering is een spelregel. Dat klinkt streng maar de Ide kan die spelregels juist gebruiken om je te helpen keuzes te maken.

  Om aan je gegevens te komen mag je echter alles inzetten, je hebt een hele programmeeromgeving ter beschikking om er aan te komen. Je kan databases benaderen, de cloud voor  kluizen of wat dan ook, je kan compileren, docker imgages maken, allemaal geen probleem. 

  Hou echter de traceerbaarheid in het oog. Gebruik daarom gegevens die in Git staan. Er is een uitzondering: **_Zet je geheimen niet zichtbaar in de sources_**. Haal ze uit een kluis, ik gebruik zelf een kluisjes in Azure, in F# kan je een functie schrijven en gebruiken die de geheimenen uit jouw kluis halen, met intellisense kan je jouw geheim kiezen en tikfouten zijn verleden tijd. Bedenk wel dat hier veiligheid boven traceerbaarheid gaat.

  Voorbeeld gebruik Secrets:
  ~~~
    // zo moet het niet
    let password = "EenGeheimWachtWoord"

    // zo is het veiliger
    let password = secret Secrets.DatHeleGeheimeWachtwoord
  ~~~  

* Inhoud van de servicebeschrijving afstemmen op de omgeving.

  Omdat je meerdere omgevingen hebt, en je verschillen per omgeving kan hebben, denk aan een andere database benaderen, andere geheimen en meer, geef je geen vaste beschrijving van een service maar geef je een beschrijving op basis van de omgeving. 
  
  Dat betekent concreet dat je een functie schrijft met een parameter, de omgeving. Het resultaat is niet de beschrijving maar een optionele beschrijving, het kan namelijk ook zijn dat de service er niet is voor een omgeving. Dat is helemaal niet vreemd denk maar eens aan een nieuwe service die geïntroduceerd is bij een nieuwe _change_ die nog niet verder is dan de **_Ontwikkel_** omgeving, in **_Productie_** is die nog niet aanwezig.

  Een simplistisch voorbeeld:

  ~~~
  // typering van de omgevingen
  type Environment =
  | DEVL
  | TEST
  | ACPT
  | PROD

  // niet een echte service typering, just an example
  type Service = {
    Content: string;
    Secret: string
  }
  
  // de functie die de service beschrijving teruggeeft op basis van de environment
  let getService env =
    let content = "omgeving onafhankelijke content"
    match env with
    | DEVL       -> Some { Content: content; Secret: secret Secrets.DevelopmentGeheim }
    | TEST, ACPT -> None
    | PROD       -> Some { Content: content; Secret: secret Secrets.ProductieGeheim }
  ~~~

* Uitbreiden beschrijving zonder aanpassing van oude beschrijvingen.

  Het kan zomaar gebeuren dat er nieuwe beschrijvingen nodig zijn omdat voor nieuwe situaties bestaande beschrijvingen onvoldoende zijn.  Die nieuwe beschrijvingen moeten gewoon toegevoegd kunnen worden waarbij de bestaande beschrijvingen nog steeds als valide worden beschouwd. Stel dat er een nieuw type **_Program_** nodig is:

  ~~~
  type SpecialProgram = {
    Id: string;
    Env: Dtap;
    SpecialProperty: Speciality;
  }
  type Program =
    | ExternScript of ExternScript
    | ExternProgram of ExternProgram
    | FileProgram of FileProgram
    // SpecialProgram wordt toegevoegd
    | SpecialProgram of SpecialProgram
  ~~~

  Als een bestaande export file van een Domein zonder SpecialProgram wordt geïmporteerd dan voldoet het aan de F# specificatie van de nieuwste **_Program_** met SpecialProgram. Natuurlijk kan een oud runtime systeem zonder SpecialProgram geen import file met SpecialProgram verwerken, om dat van te voren te bepalen wordt bij iedere export opgenomen wat de modernste constructie is waar het (ook echt) gebruik van maakt, en bij import mag dat niet nieuwer zijn dan wat het runtime systeem aankan.

* De mogelijkheid eigen templates te maken.

  Soms is het een werk alle properties van een service telkens op te geven als ze voor een bepaald type service al vastliggen of automatisch te bepalen zijn. F# heeft heel handige features om vanaf een ingevuld record een nieuw record te maken met enkele aanpassingen.
  
  ~~~
  let default ParallelProgramTest = {
    Id = "defaultId";
    Env = TEST;
    SpecialProperty = RunParallel
  }
  let ParallelImportPogramTest = { ParallelProgramTest with Id = "Import" }
  ~~~

  String interpolation in dotnet kan gebruikt worden om strings op basis van programma logica samen te stellen. Omdat je hele sources in een string kwijt kan is dat ook een veel gebruikt iets:

  ~~~
  let b = true
  let pythonScript env  =  $"""
  {DemoScriptV1.envScript env}
  {System.IO.File.ReadAllText "services/demoscript/demoscript.v2.py"}
  {if b then "// b is waar"; else "// b is onwaar"}
  // en dit is commentaar in het python script
  // en dit ook
  a = 12;
  """ // einde functie pythonScript, door de 3 dubbele quotes worden linefeeds en opmaak behouden.
  // testScript wordt een pythonScript voor omgeving TEST
  let testScript = pythonScript TEST
  ~~~
