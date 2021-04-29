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

    Ik heb het gekozen om zijn robuuste opzet maar vooral voor het afdwingen van het "single instance" runnen. Vooral met batch processen en trigger condities moet je niet perongeluk parallel draaien. Mijn ervaring leert dat het heel vervelende fouten kan opleveren die sporadisch voorkomen, nooit in test situaties maar juist in de productie omgeving.
    Door de queue van de mailbox worden de signalen serieel gemaakt en is er ook sprake van serieel uitvoeren.

  * Event handlers
    
    Mijn Eventhandlers voor signals, watch en queues en timers ondersteunen dat model door een bericht op de queue te zetten en **_niet_** de verwerking te starten.


  * Service verwerking

    Een service is een flow van programma's die iedere tijd interval, of via een ander event wordt gestart. Het starten gebeurt altijd via een bericht op de queue van de mailboxprocessor waar de service aangekoppeld is. Het is het veiligst om een child process te starten wat asynchroon wordt gestart maar op de uitkomst wordt gewacht voordat het volgende queue bericht wordt gelezen. 

    Vaak kan de service pas starten als er bepaalde condities is voldaan, bijvoorbeeld bepaalde bestanden aanwezig of iets in een database. Dat kan in de service worden opgelost door een trigger stap te combineren met de werkelijke verwerking in een "UntilError" flow. Als de trigger stapt faalt, wordt de verwerking stap niet uitgevoerd. Het hoeft natuurlijk niet zo, die trigger conditie kan ook in de verwerking zijn opgenomen, maar het is een mogelijkheid als je het originele verwerking niet wil of kan aanpassen.

    * Bewust parallel runnen
      
      Er is de mogelijkheid om steps van een flow parallel uit te voeren, dit is dan de bewuste keuze. Er is ook de mogelijkheid om bij aanwezigheid van meerdere files deze parallel te verwerken. Via een regular expression geef je aan waar de filename of filepath aan moet voldoen en hoe oud de file minimaal moet zijn, en hoe de optionele output filenaam eruit moet zien (op basis van de inputnaam).

     
    


