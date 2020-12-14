---
layout: default
title: Integratie van systemen
---
# {{ page.title }}

## Expertise

  Mijn expertise is integratie van systemen, door een [_domain driven_](https://en.wikipedia.org/wiki/Domain-driven_design) aanpak worden gegevens tussen de verschillende domeinen uitgewisseld. Bij die uitwisseling zijn vaak heel verschillende soft- en hardware technieken betrokken.  
  Dit resulteert in veel verschillende programma's, scripts e.d.  
  
  De kunst is dit ordelijk te onderhouden, op de juiste momenten te laten draaien, als de juiste invoer aanwezig is, automatisch te herstellen na problemen en dan ook zorgen dat het allemaal veilig gebeurt. 

## Welke problemen kan je voorkomen
  
  Denk aan mutaties die niet worden uitgevoerd, mails die niet verstuurd worden, boekingen die niet gedaan worden of erger: dubbel worden uitgevoerd.  

  En bij de handmatige correctie hebben medewerkers bevoegdheden nodig waarmee ze dat buiten alles om kunnen doen, een nachtmerrie voor de beveiliging en consistentie.

## Wat moet er voor gedaan worden

  Ik weet dat je naast het script of programma meer moet vastleggen in Git zoals bijvoorbeeld de _opstart (trigger) conditie_, de _variatie_ per (test)omgeving en het eventueel parallel kunnen draaien.  
  En hoe je deze sources moet vastleggen en groeperen om ze geautomatiseerd op de bestemming te krijgen waar ze hun werk gaan doen. 

  Het is niet dat je meer moet doen maar dat je van tevoren in kaart brengt en het systeem zo opbouwt dat er voldoende informatie aanwezig is om alles automatisch te doen. 

## Wat is het resultaat

  Het gaat allemaal om foutloos werkende en daarom goed geteste oplossingen, door de aanwezigheid van test-omgevingen([DTAP](https://en.wikipedia.org/wiki/Development,_testing,_acceptance_and_production)). Traceerbaar tijdens de ontwikkeling (_Git_) en tijdens draaien (_runtime_) door goede logging, die van het script maar ook die van de trigger conditie, ook of juist in geval dat die niet afgaat. De _run-time_ log zorgt voor de link naar de juiste versie van de source. 
