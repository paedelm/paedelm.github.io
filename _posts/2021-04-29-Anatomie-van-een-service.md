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

    Een service is een flow van programma's die iedere tijd interval, of via een ander event wordt gestart. Het starten gebeurt altijd via een bericht op de queue van de mailboxprocessor waar de service aangekoppeld is. Het is het veiligst om een child process te starten wat asynchroon wordt gestart maar op de uitkomst wordt gewacht voordat het volgende queue bericht wordt gelezen. 

    Vaak kan de service pas starten als er bepaalde condities is voldaan, bijvoorbeeld dat bepaalde bestanden aanwezig zijn of dat iets in een database staat. Dat kan in de service worden opgelost door een trigger stap te combineren met de werkelijke verwerking in een "UntilError" flow of een shell script te schrijven wat hetzelfde doet. Als de trigger stapt faalt, wordt de verwerkings stap niet uitgevoerd. Het hoeft natuurlijk niet zo, die trigger conditie kan ook in de verwerking zijn opgenomen, maar het is een mogelijkheid als je het originele verwerking niet wil of kan aanpassen.

    * Bewust parallel runnen
      
      Er is de mogelijkheid om steps van een flow parallel uit te voeren, dit is dan de bewuste keuze. Er is ook de mogelijkheid om bij aanwezigheid van meerdere files deze parallel te verwerken. Via een regular expression geef je aan waar de filename of filepath aan moet voldoen en hoe oud de file minimaal moet zijn, en hoe de optionele output filenaam eruit moet zien (op basis van de inputnaam).

     
    


