---
layout: default
title: CV
---
# {{ page.title }}

## Ervaring
  
  Ik heb vanaf 1981 ervaring als software ontwikkelaar bij diverse banken, als zelfstandige en met een uitstapje naar de logistieke sector ben ik de laatste 17 jaar actief geweest bij een verzekeringsmaatschappij.
  Vanaf de middelbare school (VWO-B) ben ik in 1979 geheel onverwacht met computers in aanraking gekomen door een baantje te accepteren als computer operator bij een bank. De interesse was gewekt, na een test mocht ik de opleiding tot Cobol programmeur volgen en zo ben ik in 1981 software gaan ontwikkelen in Cobol en na een cursus Assembler heb ik daar ook nog het onderhoud voor gedaan.

  Intussen heb ik in veel talen geprogrammeerd en veel systemen gezien. Van procedurele talen via OO talen tot functionele talen. Van procedureel naar declaratief. De talen evelueren en als programmeur kan je goed gebruik maken van de formele syntax om het programma correct te krijgen, dan kan meestal niet voor 100 procent dus testen is noodzakelijk. De moderne (functionele) talen zijn duidelijk beter in correctheid maar vereisen wat meer abstractie vermogen. Ik ben nog altijd enthousiast over de ontwikkelingen op dat terrein en expirimenteer daar nog graag mee. Als (technisch aangelegde) ontwikkelaar heb je ook te maken met de infrastructuur, ik heb in al die jaren heel wat ervaring opgebouwd maar ik ben wel enthousiast over de Cloud systemen die een boel rompslomp van je wegnemen en die een ontwikkelaar enthousiat maken door "Infra as Code" principes.     

## Expertise

  Mijn expertise is integratie van systemen, door een [_domain driven_](https://en.wikipedia.org/wiki/Domain-driven_design) aanpak worden gegevens tussen de verschillende domeinen uitgewisseld. Bij die uitwisseling zijn vaak heel verschillende soft- en hardware technieken betrokken.  
  Dit resulteert in veel verschillende programma's, scripts e.d.  
  
  De kunst is dit ordelijk te onderhouden, alleen gebruik maken van de externe interface van het andere domein, op de juiste momenten te laten draaien, als de juiste invoer aanwezig is, automatisch te herstellen na problemen en dan ook zorgen dat het allemaal veilig gebeurt. 

## Welke problemen kunnen ontstaan?
  
  Denk aan mutaties die niet worden uitgevoerd, mails die niet verstuurd worden, boekingen die niet gedaan worden of erger: dubbel worden uitgevoerd.  

  En bij de handmatige correctie hebben medewerkers bevoegdheden nodig waarmee ze dat buiten alles om kunnen doen, een nachtmerrie voor de beveiliging en consistentie.

## Wat moet er voor gedaan worden

  Ik weet dat je naast het script of programma meer moet vastleggen in Git zoals bijvoorbeeld de _opstart (trigger) conditie_, de _variatie_ per (test)omgeving en het eventueel parallel kunnen draaien.  
  En hoe je deze sources moet vastleggen en groeperen om ze geautomatiseerd op de bestemming te krijgen waar ze hun werk gaan doen. 

  Het is niet dat je meer moet doen maar dat je van tevoren in kaart brengt en het systeem zo opbouwt dat er voldoende informatie aanwezig is om alles automatisch te doen. 

## Wat is het resultaat

  Het gaat allemaal om foutloos werkende en daarom goed geteste oplossingen, door de aanwezigheid van test-omgevingen([DTAP](https://en.wikipedia.org/wiki/Development,_testing,_acceptance_and_production)). Traceerbaar tijdens de ontwikkeling (_Git_) en tijdens draaien (_runtime_) door goede logging, die van het script maar ook die van de trigger conditie, ook of juist in geval dat die niet afgaat. De _run-time_ log zorgt voor de link naar de juiste versie van de source. 
