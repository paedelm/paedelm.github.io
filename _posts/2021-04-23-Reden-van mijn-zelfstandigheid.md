---
layout: post
title: "Waarvoor ga ik me inzetten als zelfstandige"
date: 2020-04-22
---

## Actuele problemen

 Alom in het nieuws: storing in een systeem van de overheid, cijfers niet op tijd, bedrijf in de problemen door een hack op een essentieel systeem, gegevens gelekt en aangeboden op het dark net, en dat bijna iedere dag. Ik ben bang dat er nog heel wat systemen zijn die bij gerichte criminele aandacht flink door de mand vallen. Ze zijn daar vroeger meestal niet op ontworpen en het werd ook niet afgestraft maar dat is tegenwoordig wel anders. 

## Mijn expertise
 
  Systemen betrouwbaar maken, dat betekent ook veilig maken want onveilig is onbetrouwbaar maar onbetrouwbaar is onveilig. Om dat laatste toe te lichten:
  > Problemen leiden vaak tot handmatige aanpassingen waarvoor medewerkers ter plekke, op het systeem zelf, aanpassingen maken, dat kan vaak ongecontroleerd en kan leiden tot onbedoelde fouten of gelegenheid geven aan indringers om gebruik te maken van de reparatie mogelijkheden om iets crimineels te doen.

## Mijn methodiek in het kort vanuit een ander perspectief.
Een metafoor:
> **Een fabriekshal van 20 jaar terug**: er gaan grondstoffen in en na verwerking komen er eindproducten uit. Door de open deuren worden grondstoffen binnen gebracht en neergezet naast de eindproducten die wachten op de verschillende vervoerders. Verderop worden de producten gemaakt door specialisten die verschillende handelingen uitvoeren en dat in de juiste volgorde moeten doen. Er liggen gedetailleerde instructies. 

> **De fabriekshal nu**: Een afgesloten fabriekshal, aan de buitenkant verschillende kluizen voor het deponeren van grondstoffen en kluizen voor afnemers van het eindproduct. Het productie proces is geautomatiseerd, specialisten werken nu op de research afdeling, zij ontwikkelen en testen aanpassingen in het productie proces voordat het in de fabriek wordt toegepast.

Er mag tegenwoordig niks mis gaan, invoer, uitvoer en het proces worden beschermd tegen sabotage, spionage en diefstal, er blijft ook niks van waarde achter op de plaats van produceren. Ontwikkelen van het proces gebeurt ergens anders, het bestaande proces wordt in zijn geheel vervangen door een geteste nieuwe versie. De productie plaats is afgesloten, maar zou iemand toch doordringen dan is er nog steeds niks van waarde te vinden. En die glimmende apparaten? Die geven hun geheimen niet prijs!
## Leuk verhaal maar nu concreet, wat is mijn methodiek in het echt?

 * Het beheer moet als allereerste van de server af.
 
   Alle aanpassingen moeten vanuit Git (sourcecontrol) plaats vinden om traceerbaar en controleerbaar te zijn. Vanuit Git ontstaat de configuratie die in zijn geheel op de runtime omgeving wordt gezet.
   
 * Opdelen in domeinen.
   
   Je deelt het systeem op in stukken die een logische eenheid vormen. Binnen zo'n domein zijn er geen beveiligings drempels. Naar buiten toe is het domein een bastion waar niemand bij mag. Dit geldt voor de bronnen in Git en ook voor de runtime omgeving.
   
 * Het domein volledig beschrijven.

   Omdat alles vanuit git moet doen, moet de domein configuratie alles bevatten wat nodig is om probleemloos te draaien en te beheren en worden er speciale eisen aan gesteld. 
   * Dus niet alleen de sources van programma's of scripts,
   * maar ook hun manier en hun moment van starten,
   * de gegevens om externe resources zoals databases, kluizen, externe sites, etc te benaderen.
   * Overige gegevens die nodig zijn om de configuratie te genereren, denk aan compileren.
   * Overige gegevens die nodig zijn om te gebruiken in de runtime omgeving. Er is een formele beschrijving voor de aanlevering van de configuratie export, daar zit een blob property bij waar je de content van een directory tree kan opnemen.
   * Heel belangrijk is dat geheimen zoals passwords en sleutels niet in Git worden opgenomen maar vanuit een kluis worden ingebracht.
   * Test scripts om controles uit te voeren
   * Aanpassingen (changes) registreren. 
   * Verschillende versies bijhouden, en die koppelen aan hun change.
   * Eventuele dockerfile voor de configuratie van het runtime systeem.
   * Actief maken van het sourcesysteem: vanuit de sources wordt de domein configuratie gegenereerd. Denk daarbij aan genereren voor verschillende (test) omgevingen. De juiste versie wordt bepaald op basis van de status van een change. Ter verduidelijking: zo'n change zit natuurlijk niet direct in productie maar doorloopt waarschijnlijk de cyclus van Development via eventuele extra test omgevingen naar Productie.

 * Genereren van een omgeving specifieke versleutelde export.
  
   Het generatie proces roept jouw configuratie items aan met de environment parameter, het zorgt voor de juiste combinatie van versie en environment op basis van de status van de change. Het doet allerlei algemene controles en jij kan voor ieder item je eigen controles programmeren. De laatste controle is jouw test, en als die goed gaat, worden de versleutelde Json-files geproduceerd.  

 * Changes vooruit rollen of terug rollen.

   Soms wil je een aanpassing in productie nemen of naar een volgende test-omgeving laten gaan. Daarvoor verander je de status van de change naar die nieuwe omgeving en genereer je opnieuw de export files.

   Er is geen verschil in handelingen om vooruit of weer achteruit te gaan als blijkt dat er iets over het hoofd is gezien. Het idee om dit vanuit Git te doen is de traceerbaarheid.


## Beveiligd uitvoeren:
> Weet jij hoelang het duurt voordat je erachter komt dat er een script (shell, python nodejs etc..) is aangepast? Daar moest ik aan denken, toevallig is het ook kort geleden [**gebeurd**] (https://tweakers.net/nieuws/180646/criminelen-stalen-inloggegevens-van-ontwikkelaars-via-devtool-bash-uploader.html)


Hoe mooi ook zo'n "Beheer vanuit Git" systeem als je zo de scripts kan aanpassen, werkt het niet. Mijn antwoord daarop is: versleutel de configuratie en zorg ervoor dat je het script nergens opslaat, indien toch nodig, kan het er vlak van te voren opgezet worden en na draaien worden verwijderd, telkens weer. 

## Geheimen ontdekken in de configuratie:

Al in Git is het zaak al de geheimen (wachtwoorden sleutels etc..) vanuit een kluis op te halen of je scripts zo inrichten dat ze op run-time uit de kluis worden gehaald.

## Opslag en privacy van gegevens

> Daar mee kom je liever niet in het nieuws, **_jouw gegevens aangeboden op het dark net._**

* Extern en versleuteld opslaan.
  
  Dat heeft mijn voorkeur, in een soort datakluis, dat maakt het moeilijk voor een indringer van jouw runtime systeem iets te vinden van waarde en het betekent dat de server inwisselbaar wordt.

* Handige oplossingen voor het (**niet**)aanpassen van bestaande software voor het gebruik van versleutelde bestanden.

  Voor het beveiligd uitvoeren is er al een tussenlaag die jouw script of programma uitvoert. Deze tussen laag heeft ook faciliteiten om jouw script aan te roepen en via stdin stream het ontcijferde bestand te geven of vlak van te voren het bestand ontcijferd neer te zetten en na afloop te verwijderen. De uitvoer kan via de stream, stdout of via de persistente variant versleuteld worden en de leesbare versie wordt dan verwijderd. Het maakt voor de tussenlaag niet zoveel uit of het versleutelde bestand op schijf of vanuit een datakluis komt. 

* Log informatie kan gevoelig zijn en behoort niet op het runtime zelf te worden opgeslagen. En uiteraard is ook hier versleuteling de norm. 


## Isoleren van het systeem
* Gebruik geen interactieve shell meer.

* Zet geen poorten open, alle software die toch luistert op een poort, moet kritisch bekeken worden.

* Import gegevens van buiten het domein, een veilige oplossing.
 
  Als je gegevens krijgt van buiten het domein, maak dan een aparte locatie, bijvoorbeeld in de cloud, geef de eigenaar van de gegevens een schrijfsleutel en haal de gegevens vanuit die locatie naar jouw systeem om te verwerken. Vanwege de beveiliging, versleuteling, de hulpmiddelen en de documentatie is dat een makkelijke manier.



## Samenvatting

Samenvattend kan je stellen dat traceerbare wijzigingen in een smart configuratie systeem en waardoor je geautomatiseerd kan beheren en door de tussenlaag je eenvoudiger over kan gaan naar versleutelde data eventueel buiten de server. Je minder (geen) noodzaak hebt om op de server te komen, en wie op de server komt, niks kan vinden van waarde. Daardoor ben je ook minder interessant voor de crimineel, het kost te veel moeite met weinig zicht op crimineel-resultaat. Voor de betrouwbaarheid heb je het voordeel dat alles terug te vinden is in Git, er getest naar de volgende omgeving wordt gegaan, en door de koppeling van een versie van de configuratie aan een change, de rollforward en rollback van de change de juiste versies raken. 

## Mijn rol
 Het bewust maken van deze manier van werken, het gezond verstand stuk, en bij de implementatie fase, het opzetten van het smart configuratie systeem en het omgaan en beschrijven van flows voor de tussenlaag die van jouw scripts en programma's processen maakt die continue draaien en jouw flow op het juiste moment aanroepen. Mijn uitgangspunt is dat je (het scrum team) het uiteindelijk zelf moet kunnen onderhouden, en gezien alle informatie over het systeem terug te vinden is in Git, en bewezen actueel is, moet dat geen probleem zijn. 

