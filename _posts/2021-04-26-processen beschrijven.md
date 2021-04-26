---
layout: post
title: "Beschrijven van processen om ze geautomatiseerd te draaien"
date: 2021-04-26
---

## Achtergrond

 In het kader van een betrouwbaar en veilig IT domein, is het idee om de processen in Git te beschrijven en met die beschrijving ze ergens anders geautomatiseerd te laten draaien. Vanwege dat automatisme zal de beschrijving getypeerd moeten worden. Eenduidigheid is nodig, je hebt geen mogelijkheid meer om te vragen "wat bedoel je". Er zijn heel wat eisen te stellen aan de beschrijving, denk aan de mogelijkheid van uitbreiden van het schema zonder oude beschrijvingen te herschrijven, ondersteuning in een IDE zoals intellisense, maken van eigen templates voor bekende proces patronen, eenvoudige syntax en nog meer. 
 
 Mijn keuze is niet op een export beschrijving gevallen maar op een programmeertaal die bekende export formaten kan produceren. Een programmeertaal omdat daar alles inzit voor hergebruik, conditioneel genereren, indelen in sourcefiles, gebruik van intellisense, pre-processing zoals compileren, controleren zoals testen en nog veel meer. Uiteindelijk ben ik op F# terecht gekomen het heeft de voordelen van een dotnet omgeving met zijn uitgebreide library, het heeft een eenvoudige syntax, het heeft string interpolatie voor templates, condities zijn expressies en geen statements, en het allerbelangrijkste is de [Discriminated Union](https://fsharpforfunandprofit.com/posts/discriminated-unions/). C# kwam in de buurt maar heeft nog niet alles wat F# kan. Ik heb me beperkt tot DotNet en daarbinnen gericht op sterk getypeerde talen met mogelijkheid tot reflectie. 

## Wat moet je beschrijven van een proces?

 Een proces bestaat uit een programma flow, de main Step, die periodiek wordt gestart om zijn werk te doen. Zo'n flow is een recursieve structuur: de flow bestaat uit 1 of meerdere stappen en een stap is een flow of een task. Zie de definitie in F#:
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
type Process = {
    Name: string;
    Program: Step;
    TriggerConditionRec: TriggerConditionRec
}

 ~~~
Je bent vrij een flow samen te stellen van van verschillende programma's zoals je dat ook zou doen als je er een shell script van maakt. Het flowtype "UntilError" geeft aan of je de flow wil stoppen bij de eerste stap die fout gaat, dit zal je zeker gebruiken als je jouw proces voorwaardelijke start bijvoorbeeld bij de aanwezigheid van een bestand, is het niet aanwezig of nog niet oud genoeg dan stopt het proces voor deze periode. Met de watch parameter kan je de wacht periode bekorten om sneller te reageren op bijvoorbeeld de aanwezigheid van een bestand. Er zijn meerdere flowtypes.

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
De essentie hiervan is dat ExternScript als child process wordt opgestart, met de mogelijkheid tot redirect van stdin en stdout en ook de mogelijkheid om een eigen directory tree door te geven. ExternProgram is een DLL aanroep dus kent het ook geen redirects, deze mogelijkheid is gemaakt voor snelle checks en enigzins gevaarlijk omdat het niet in een child process draait.

FileProgram is een koppeling van bestanden aan een Program, een pseudo Program, hiermee kan je binnen het proces toch een aantal bestanden parallel verwerken.

Hieronder volgt een definitie voorbeeld:
~~~
let pythonScript env  =  $"""
{DemoScriptV1.envScript env}
{System.IO.File.ReadAllText "services/demoscript/demoscript.v2.py"}
"""

let Proces name env = 
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
Ter verduidelijking: Er is geen enkelvoudige source maar de source is altijd afhankelijk van de omgeving waarin het draait. Dus als je OTAP omgevingen hebt dan heb je een viervoudig script, die van O, T, A en P.
Daarom schrijf je een functie die als parameter de omgeving mee krijgt, en jij dus de mogelijkheid hebt om te gaan varieeren per omgeving. Daarnaast kan je aangeven of je wel een proces hebt voor die omgeving. Dat heet een option, je geeft terug "Some Value" of "None"

 Als eerste wordt hier het pythonScript aangemaakt. Het bestaat uit environment variabelen en een python source die ingelezen wordt vanuit een file. Er wordt van het zo ontstane python script een Task aangemaakt met een Program van het type ExternScript met als runner python waaraan ons script doorgegeven. Dit Program wordt gebruikt in ons Process en dat wordt alleen gegenereerd als we in DEVL of TEST omgeving zitten. In de overige omgevingen wordt None geproduceerd.
