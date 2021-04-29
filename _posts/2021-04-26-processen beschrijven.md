---
layout: post
title: "Beschrijven van processen om ze geautomatiseerd te draaien"
date: 2021-04-26
---

## Achtergrond

 In het kader van een betrouwbaar en veilig IT domein, wordt het ontwikkelen en uitrollen van de processen beheerd vanuit Git. In de bron wordt het haarfijn beschreven met als doel de juiste versleutelde versie te exporteren. Het **_runtime systeem_** gebruikt die om de processen als beveiligde **_Services_** te laten draaien.

 
 Omdat er geen hand meer aan te pas komt, moet de beschrijving formeel en volledig getypeerd worden. Vanwege het volledige beheer krijg je te maken met versies en omgevingen wat resulteert in verschillende variaties van dezelfde service. 

 De keuze voor wat je gebruikt om te beschrijven, is een belangrijke. Naast de eisen zijn er ook veel wensen:   **_makkelijk te onderhouden_**,  **_ondersteuning bij het invoeren_**, dat **_uitbreiding of wijziging van de vast te leggen gegevens_ zonder verplichte migratie** en**_eigen templates te maken_ voor veel voorkomende patronen** , om maar een een aantal te noemen.
  
 Om het wiel uit te vinden is _**on**verstandig_ dus ga je op zoek naar iets bestaands waarvan je weet dat het goed werkt, support aanwezig is en wordt onderhouden. 
 
 Dus kom je op een een programmeertaal, met sterke typering en een eenvoudige syntax en een sterk eco-systeem. Er moeten records getypeerd kunnen worden en de velden van de records moeten de bekende primitieven aan kunnen, zoals string, integer, date, time, boolean en nog meer. Een record moet ook een record als veld kunnen hebben en ook zichzelf, en heel belangrijk is ook dat je generieke types kan gebruiken waarvoor je een specifiek type kan invullen. Natuurlijk moet de recordstructuur te exporteren zijn naar Json of Xml of iets anders, als de import maar hetzelfde oplevert als de recordstructuur voor de export. Die controle is belangrijk en moet uitgevoerd kunnen worden. 
 
Een programmeertaal dus en niet het handmatig onderhouden van het exportformaat. Omdat met een programma hergebruik, conditioneel genereren, indelen in sourcefiles, gebruik van intellisense, pre-processing zoals compileren, controleren zoals testen en nog veel meer, mogelijk is. Uiteindelijk ben ik op F# terecht gekomen het heeft de voordelen van een dotnet omgeving met zijn uitgebreide library, het heeft een eenvoudige syntax, het kan goed de types afleiden, het heeft string interpolatie voor templates, condities zijn expressies en geen statements, en het allerbelangrijkste is de [Discriminated Union](https://fsharpforfunandprofit.com/posts/discriminated-unions/). C# kwam in de buurt maar heeft (nog) niet alles wat F# kan. Ik heb me alleen beperkt tot DotNet en daarbinnen gericht op sterk getypeerde talen met mogelijkheid tot reflectie. 

## Wat moet je beschrijven?

 Je beschrijft de services die in het domein gaan draaien en de levensloop van aanpassingen in het domein waar versies van de service aan gekoppeld zijn.
 
### De service
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
**_Program_** is zo'n generieke typering, het kan een **_ExternScript_**, **_ExternProgram_** of **_FileProgram_** zijn.

**_ExternScript_** wordt als child process opgestart, met de mogelijkheid tot redirect van stdin en stdout en ook de mogelijkheid om een eigen directory tree door te geven.

**_ExternProgram_** is een DLL aanroep dus kent het ook geen redirects, deze mogelijkheid is gemaakt voor snelle checks en enigzins gevaarlijk omdat het niet in een child process draait.

**_FileProgram_** is een koppeling van bestanden aan een **_Program_**, eigenlijk een _pseudo_ **_Program_**, met property "InputFileSelect" geef je aan dat voor ieder bestand dat voldoet aan die conditie, het programma wordt opgestart. Hiermee kan je binnen de service toch bedoeld parallel verwerken.

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

Ter verduidelijking: De source bedient alle mogelijke omgevingen het bevat een functie die als parameter de omgeving mee krijgt en die als resultaat het item voor de bewuste omgeving terug geeft.  Als het source systeem gaat genereren, dat doet het als het runt, worden jouw functies aangeroepen met de juiste versie voor de juiste omgeving, en dat is gebaseerd op het _change_ systeem, waar iedere versie van een service is gekoppeld aan een _change_. En de levensloop van de _change_ van Development tot Productie loopt. Als de _change_ wisselt van omgeving moet er opnieuw gegenereerd worden. Je hebt dus de mogelijkheid variatie aan te brengen op basis van de omgeving, of zelfs geen service te exporteren. Dat heet een option, je geeft terug "**_Some Service_**" of "**_None_**"

 Als eerste wordt hier het pythonScript aangemaakt. Het bestaat uit environment variabelen en een python source die ingelezen wordt vanuit een file. Er wordt van het zo ontstane python script een Task aangemaakt met een Program van het type ExternScript met als runner python waaraan ons script doorgegeven. Dit Program wordt gebruikt in ons Process en dat wordt alleen gegenereerd als we in DEVL of TEST omgeving zitten. In de overige omgevingen wordt **_None_** geproduceerd.

### _Changes_
  
   Bij iedere functionele wijziging wordt een _change_ aangemaakt. Als een service geraakt wordt door die _change_, wordt er een nieuwe versie van gemaakt en gekoppeld aan de _change_. Ontstaat er een nieuwe service dan wordt dat versie 1 en gekoppeld aan de _change_, wordt een service overbodig dan komt er toch een nieuwe versie maar die geeft een **_None_** terug (zie boven).
   
   Er zijn wat controles nodig, verschillende versies van een service mogen niet naar dezelfde _change_ verwijzen en iedere versie is gekoppeld aan een bestaande _change_, die controles worden automatisch en zelfs deels door de compiler en de Ide uitgevoerd.
   
   Door de _change_ van omgeving te laten veranderen worden automatisch de juiste versies van de services voor hun omgevingen gegenereerd. Die omgevingen hebben een volgorde, bijvoorbeeld OTAP, van Ontwikkeling via Test, Acceptatie naar Productie. De nieuwste versie begint bij O. Er mag geen nieuwere versie in een omgeving verder in de lijst staan. Dus als versie 3 in P staat dan is versie 2 niet meer actief. 

   Als een _change_ uiteindelijk in **_Productie_** komt, kunnen oude versies verwijderd worden, ze worden nooit meer in een export meegenomen. Je wordt niet gedwongen maar het is wel verstandig om ze te verwijderen om zoek resultaten op je source tree niet te vertroebelen met resultaten uit files die er niet toedoen.

   Een _change_ hoeft niet altijd voorwaarts te gaan, je wilt soms terug bij problemen. Als van A naar P geen feest is, kan je de situatie redden door van P naar A te gaan.


## Waar ik rekening mee gehouden heb bij het beschrijven.

* De beschrijving moet na import gelijk zijn aan wat het voor export was.
  
  De beschrijving van het domein wordt in zijn geheel geëxporteerd en ter controle geïmporteerd, ze moeten dan gelijk zijn. Wat het export formaat is doet niet ter zake maar in de praktijk zal het versleutelde json zijn. Een eis is dat het alleen met een sleutel te lezen is.

* Je moet er alles in kwijt kunnen.
  
  Voorlopig kan alles er in behalve functions. Waar het source systeem met functions meerdere omgevingen bedient, ligt bij het export formaat de omgeving vast. Het is dus een export voor die bewuste omgeving.Omdat je byte arrays kan exporteren is al het exotische mogelijk. Ik heb daar al op voor gesorteerd door een directorytree(_ContentZipOfProgDir_) als optie op te nemen bij een _ExternScript_. Als voorbeeld hiervoor gebruik ik een java programma die voor exporteren wordt gecompileerd en waarvan de gecompileerde jar-file via de _ContentZipOfProgDir_ property mee komt in de export. 

* Optimaal gebruik maken van de Ide.

  Ik gebruik VisualStudio Code met plugin Ionide voor F# om intellisense te activeren en op je fouten te wijzen. Maar ook het python script, java programma of welke source dan ook, de Ide herkent het meestal wel of er is wel een plugin voor te krijgen en je hebt ondersteuning van de Ide bij het schrijven daarvan.

  Soms moet je kiezen: de source van een script in een string constante met interpolatie of in een file met de juiste extensie met het voordeel van Ide ondersteuning. Gelukkig kan je beiden in F#, je kan het ook combineren het omgeving afhankelijke stuk in een string en het vaste stuk in de file.

* Testen en controleren

  Door te kiezen voor een actief systeem, jouw code wordt aangeroepen om iets te exporteren, is het ook makkelijk om controles in te bouwen, zeg maar unit tests. De eind controle probeer ik af te dwingen door jouw te vragen voor iedere versie van de service een script te leveren om test gegevens klaar te zetten, dat _test-initialisatie-script_ wordt gecombineerd in een UntilError flow met de service flow. Het resultaat is een eenmalige test van jouw service, als die fouten geeft, stopt de export.

* Meerdere versies van services bijhouden en die koppelen aan de juiste omgeving.
  
  Je hebt meerdere versies nodig dat wordt duidelijk als je start met de ontwikkeling van een _change_. Dat doe je natuurlijk niet direct in de productie omgeving, je mag pas naar productie als er getest is, dus tot je change in productie is, heb je minimaal 2 versies te onderhouden.  

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

* Controle op gegenereerde export file(s).

  Soms pas je een concreet gegeven aan en soms een algoritme om iets te bewerkstelligen. Je hebt meestal wel een idee wat zo'n verandering teweeg zou moeten brengen. 

  Zorg ervoor dat de gegenereerde export files ook in Git komen te staan. Het is dan duidelijk na generatie wat er veranderd is. Doe jij iets waarvan je verwacht dat het alleen invloed heeft op jouw development omgeving en je ziet ineens dat er een wijziging is ontstaan in de productie export file dan is er wat voor je uit te zoeken. 

* Mogelijkheid tot rapportage(s)

  Je onderhoudt meerdere versies van de services, versies die in geen enkele omgeving actief zijn, vervuilen jouw source systeem, daarom is het handig die te verwijderen of te archiveren (Git archiveert ook). Je kan allemaal handige functies schrijven om deze informatie boven tafel te krijgen, en direct op de bron zelf en niet op iets wat afgeleid is.

* Terugdraaien van een _change_.

  Dat is iets wat niet anders is dan het veranderen van de omgeving van de _change_. Zette je hem van **_Acceptatie_** naar **_Productie_** en het blijkt toch niet naar wens te gaan, dan zet je hem terug van **_Productie_** naar **_Acceptatie_**. Allemaal traceerbaar in Git. Uiteraard moet je nog wel bedenken wat dat betekent voor de state in resources zoals een database of een message systeem.

* Makkelijk zoeken.

  Probeer alles van het domein binnen 1 directory tree te doen.

  De Ide heeft goede zoek mogelijkheden binnen een directory tree, het geeft je voordelen, tenzij je het bewust anders doet, zou ik me hier aan houden. Voor de rest is een logische opbouw van de tree prettig voor wie het domein moet onderhouden. En doe je iets wat afwijkend lijkt met een speciale bedoeling, beschrijf die bedoeling dan en zorg ervoor dat iemand de beschrijving vindt. En alles wat je schrijft, moet je in die ene directory tree zetten, dan is alles traceerbaar.

* Versies van Tests en Test bestanden

  De testen lopen met de versies mee. Het klinkt logisch want wil je een _change_ in productie zetten dan moet die getest zijn. Maar net als de service zelf, blijft de oude versie van de test aanwezig totdat de nieuwe versie in **_Productie_** gaat.