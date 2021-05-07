---
layout: post
title: "Anatomie van een service"
date: 2021-04-29
---

## Achtergrond

  In het kader van betrouwbare en veilige IT systemen worden de systemen in domeinen opgedeeld en worden de processen binnen het domein als service uitgevoerd door een runtime systeem.

  Het runtime systeem heeft als doel de processen beheerst uit te voeren, en dat periodiek te doen vanuit een versleuteld bron bestand om gegarandeerd de juiste programma's te starten.

## Hoe werkt het runtime systeem?

  * Algemeen

    Het programma is de service-container, het is dan ook bedoeld om services te draaien en het gebruikt daarvoor de [mailbox processor van F#](https://fsharpforfunandprofit.com/posts/concurrency-actor-model/).

  * Serieel runnen

    Ik heb het gekozen om zijn robuuste opzet en voor het afdwingen van het "single instance" runnen. Door gelijktijdig optreden van trigger events kunnen batch processen per ongeluk parallel draaien. Mijn ervaring leert dat het heel vervelende fouten kan opleveren die sporadisch voorkomen, nooit in test maar altijd in de productie situatie.

    Door de queue van de mailbox worden de signalen serieel gemaakt en kan je serieel uitvoeren afdwingen.

  * Event handlers
    
    Mijn Eventhandlers voor signals, watch events, queues en timers zetten **alleen** een bericht op de mailbox queue. Ze zullen **_nooit_** een verwerking starten.


  * Service verwerking

    Een service is een flow van programma's die iedere tijd interval, of via een ander event wordt gestart. Het starten gebeurt altijd via een bericht op de queue van de mailboxprocessor waar de service aangekoppeld is. Het is mogelijk om "in process" een dotnet dll aan te roepen, dat zou kunnen om even snel het bestaan van een file te checken, maar het is veiliger om een child process te starten, voor beiden geldt dat op de uitkomst gewacht wordt voordat het volgende queue bericht wordt gelezen. 

    Vaak kan de service pas starten als er bepaalde condities is voldaan, bijvoorbeeld dat bepaalde bestanden aanwezig zijn of dat iets in een database staat. Dat kan in de service worden opgelost door een trigger stap te combineren met de werkelijke verwerking in een "UntilError" flow of een shell script te schrijven wat hetzelfde doet. Als de trigger stapt faalt, wordt de verwerkings stap niet uitgevoerd. Het hoeft natuurlijk niet zo, die trigger conditie kan ook in de verwerking zijn opgenomen, maar het is een mogelijkheid als je het originele verwerking niet wil of kan aanpassen.

    * Bewust parallel runnen
      
      Er is de mogelijkheid om steps van een flow parallel uit te voeren, dit is dan de bewuste keuze. Er is ook de mogelijkheid om bij aanwezigheid van meerdere files deze parallel te verwerken. Via een regular expression geef je aan waar de filename of filepath aan moet voldoen en hoe oud de file minimaal moet zijn, en hoe de optionele output filenaam eruit moet zien (op basis van de inputnaam).
  
  * Input en Output streams
    
    Bij een service die als child process gestart wordt kan de input stream **_stdin_** gevoed worden door een filestream. Bij een versleutelde file kan door opgeven van de sleutel de stream ontcijferd doorgegeven worden. Voor de output stream **_stdout_** geldt iets soortgelijks, de output stream kan worden opgevangen en weggeschreven naar een file of bij het doorgeven van een sleutel kan het een versleutelde file zijn.

    Met deze mogelijkheden kan je bestaande scripts versleutelde files laten verwerken of aanmaken. Dat kan door van de streams gebruik te maken of het bestaande programma vooraf te laten gaan door een stap om te ontcijferen en te laten volgen door een stap die versleuteld. Het maken van zo'n pre- en post- stap kan met unix utility "cat" en de hierboven genoemde redirection van **_stdin_** en **_stdout_** en het opgeven van een sleutel.

    Let wel op dat de sleutels niet in een onversleutelde source terecht komen maar dat ze ergens vanuit een kluis worden gehaald.

  * Doorgeven van geheimen
    
    Een script wat van beschermde resources gebruik maakt kan de sleutels via omgevingsvariabelen ontvangen. Omdat de source van het script nu zelf versleuteld is kunnen die gegevens tijdens generatie ook vanuit de kluis in het script gezet worden. 
    
    Let wel op geheimen die door de service container in de "environment" worden gezet door alle services van het domein die daarna worden gestart, kunnen worden gebruikt. Dat moet geen probleem zijn want binnen een domein mag iedereen bij elkaars spullen. Maar in het algemeen is isoleren toch verstandig dus is het beter de geheimen alleen aan het child proces te geven.

    Er is de mogelijkheid om via **_stdout_** opdrachten aan de service container te geven om omgevingsvariabelen te zetten die dan door een andere service kunnen worden gebruikt. 

  * Aanmaken van een programma directory

    In de configuratie van een programma, wat een onderdeel is van een service, is het mogelijk om de inhoud van een directory tree op te nemen en die voor het runnen van het programma op een opgegeven plek in het file systeem te zetten. Het is niet de bedoeling dat in die directory state wordt bijgehouden, het is alleen bedoeld voor readonly gebruik of eventueel tijdelijke gegevens voor de duur van 1 verwerking.
     

     
    


