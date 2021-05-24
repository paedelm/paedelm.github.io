---
layout: post
title: "Hoe maken we onze oude IT systemen betrouwbaar en veilig?"
date: 2021-05-24
---
## Wat is er aan de hand
 Er is de laatste tijd een toename van berichten in de Media over computer storingen, gelekte gegevens, en gevallen van ransomware. 
 Het lijkt wel duidelijk dat computersystemen vaker het doelwit zijn van criminelen. 
 Gaat het hier om wat oudere, misschien verwaarloosde systemen? Is dit het topje van de ijsberg en volgen er meer meldingen?   

## Welke problemen geeft dat

  We zijn afhankelijk van onze computersystemen, bij uitval ligt vaak een groot gedeelte van de bedrijfsvoering stil. Als je slachtoffer bent van ransomware onstaat ook het dilemma of je losgeld gaat betalen. Bij gestolen gegevens maak je kans voor nalatigheid een boete te krijgen.
  De aandacht in de Media is geen goede reclame voor je bedrijf.  
  Het kan ook maatschappij ontwrichtend zijn als bijvoorbeeld een nutsbedrijf geraakt wordt. Alle voorbeelden zijn de laatste tijd wel in het nieuws geweest.

## Oorzaken vanuit het verleden

  ![](/kwetsbare-systemen.jpg)

### Als halfautomatisch systeem ontworpen

 Het ontwerp ging vroeger uit van zoveel mogelijk geautomatiseerde processen. In normale gevallen lukt dat, maar bij een lichte verstoring was een handmatige correctie gebruikelijk. Tegenwoordig merken we dat toegang voor handmatig ingrijpen gebruikt wordt voor het aanvallen van een systeem. Eenmaal toegang gekregen heeft men vaak (te) grote bevoegdheden en is er lastig te traceren wat voor acties er zijn ondernomen. Een goed bedoelende specialist kan onbedoeld het systeem ondermijnen, die andere specialist, de hacker,  is er op uit, dat merk je direct of je komt er pas (veel) later achter.
 
### Meerdere disciplines zijn verantwoordelijk

 Wat vroeger ook een rol speelde: er waren twee disciplines verantwoordelijk voor een IT systeem, de ene schreef de software en de andere rolde het uit. Dat resulteerde nogal eens in incomplete gegevens bij de overdracht, bij het over te dragen programma hoort ook extra informatie zoals de voorwaarde waaronder het moet draaien, parameters die afhankelijk zijn van de omgeving waarin het gaat draaien en nog meer. Een handleiding moest uitkomst bieden. Maar het is een kunst om die te maken, en dat is meestal niet de hobby van een software ontwikkelaar.
 Het IT systeem werd ook niet in zijn geheel overgedragen maar het was een set onderdelen, ieder met zijn eigen instructies. Installatie gaat dan bijna nooit in één keer goed, met als gevolg handmatige correcties en (te)korte testen om de goede werking te controleren. Men is (te) snel tevreden met de werking waardoor er soms vervelende subtiele fouten onopgemerkt blijven.

### Toegankelijk maken van de server voor andere systemen

 De systemen draaien nooit helemaal geïsoleerd, er zijn meestal gegevens van anderen systemen nodig. Bij zo'n overdracht was er meestal direct contact tussen de servers van beide systemen.
 Hiervoor wordt een poort open gezet, die goed beveiligd moet worden. Beveiligen betekent in dit geval zorgen dat je met de laatste security patches werkt. Dat wil vaak misgaan. Veel bedrijven wachten te lang met het aanbrengen van die patches. Dit komt regelmatig in de Media.  

### Waardevolle gegevens zijn zichtbaar aanwezig op de server van het systeem  

 Toegang tot de server van een systeem via de shell geeft bij die oude systemen ook zicht op de inhoud van scripts, configuratiebestanden en databestanden en meestal de mogelijkheid om het ook te wijzigen.
 Andere mogelijkheden zijn het stelen van gegevens en achterhalen van geheimen zoals inlog gegevens van een database. 

### De server van het systeem is moeilijk vervangbaar

 Doordat essentiële informatie, die nergens anders staat, op de server van het systeem blijft staan, kan je de server niet zo makkelijk door een ander vervangen.
 Een kapotte server, of gegijzelde server, vereist specialistisch werk en kost tijd.

## Wat kan je er aan doen?

### Inleiding
  
#### _Bescherming tegen ongewenste wijzigingen_

  Het uiteindelijk doel is een systeem te hebben wat betrouwbaar werkt en wat haar gegevens beschermd tegen diefstal. Uitgaande van een betrouwbare situatie, moet na een wijziging het systeem opnieuw betrouwbaar zijn. Daarvoor moeten de aanpassingen traceerbaar zijn en gecontroleerd zijn door minimaal 2 geautoriseerde personen en zijn getest volgens jouw normen. Dus op de server even een aanpassing doen, is er niet meer bij. Met dit uitganspunt is interactieve toegang tot de server van het systeem niet nodig waardoor de veiligheid enorm toeneemt.  

#### _Versleutelen van waardevolle onderdelen_ 
  
  Toch kan je niet uitsluiten dat iemand alsnog op de server kan komen en daar proberen gegevens te stelen, scripts aan te passen of het systeem te gijzelen. Dat is niet erg als al die belangrijke gegevens al door jou zelf zijn versleuteld en door jou te reproduceren zijn.

#### _Dicht zetten van poorten_

  Om op de server geen open poorten te hebben, daardoor minder op te vallen en vrij te zijn een andere server in te zetten, zorg je ervoor dat niemand direct contact hoeft te maken met jouw server. Hoe wissel je dan gegevens uit met een ander domein? Dat kan via een derde, betrouwbare partij, bijvoorbeeld een cloud provider, waar je een storage container huurt en die gebruikt om data uit te wisselen. De beveiliging en beschikbaarheid daarvan, transport (Https) en opslag (versleuteld) is in orde, beter waarschijnlijk dan je zelf kan bereiken. En tegenwoordig bieden ze je de mogelijkheid om events te krijgen, dat kan als Https client daar hoef je geen Https server (meer) voor te zijn. 
  
#### _Persistente gegevens buiten de server opslaan_ 
  
  Je kunt de server van jouw systeem zo voor een ander inwisselen als het je ook lukt om alle persistente gegevens buiten de server op te slaan. En dat kan door gebruik te maken van een storage provider, veiligheid en beschikbaarheid is dan in orde, alleen de bestaande processen moeten daar nog mee omgaan. Dat wordt opgelost door _de service die het proces beheert_, die zorgt dat de gegevens _just in time_ beschikbaar zijn op het moment dat het proces wat de gegevens gaat verwerken kan starten en de gepersisteerde resultaten van het proces, verplaatst naar de storage provider.  

#### _Installatie procedure vereenvoudigen_

  Het domein (zie opbreken in domeinen) is eenvoudiger te installeren op een server als de domeinbeschrijving maar uit één configuratiebestand bestaat.

#### _Enkele aanroep start het domein_ 

  Het is de bedoeling om een server te hebben waar een programma draait met deze pseudo aanroep:  
  
  - runITDomein <versleutelde domeinbeschrijving>  

  En daarmee wordt het domein met één commando in de lucht gebracht. Hierdoor wordt het eenvoudiger een nieuwe versie van het domein te draaien. Je kunt ook met één klap de normale verwerking vervangen. Dat is te gebruiken in situaties waar een conversie nodig is voordat een nieuwe versie van het domein actief wordt. 

#### _Een service per proces_

  Op die domeinserver draait voor ieder proces een nooit eindigende service die het proces telkens start als de voorwaarde daarvoor geldig is.
   
  Iedere wijziging betekent een nieuwe domeinbeschrijving, met nieuwe, gewijzigde of verwijderde services.

  Een nieuwe domein beschrijving onstaat alleen als deze getest en goed bevonden is.
### Werkwijze

#### **_Breek het systeem op in domeinen_**

  Ieder domein van het systeem staat op zijn eigen server en heeft zijn eigen Git repository. Het idee is dat iedere verandering terug te vinden is in de repository. Het onderhoud op de repository gebeurt door een team met als doel dat iedere wijziging door minstens 4 ogen is gecontroleerd. Als er onderdelen zijn van het systeem waar het team geen verantwoordelijkheid voor heeft dan verhuist dat stuk naar een ander domein, en zal er een formele interface worden afgesproken. Verdere regels voor het opbreken kunnen van technische en organisatorische aard zijn. Ieder domein heeft zijn eigen beheer cyclus.

#### **_Beheer het IT domein als een geheel_**
 
 Wat is een IT domein concreet? Het bestaat uit processen die, als ze mogen en kunnen, periodiek gestart worden. Er is een service per proces die de voorwaarden in de gaten houdt en het proces dan start. Het proces zelf bestaat uit één of meerdere programma stappen en zo'n stap is een programma taak of zelf weer een flow van stappen. De stap geeft altijd een uitkomst: het is goed of fout gegaan.  

   In de flow kan je spelen met die uitkomst. Je hebt flows die de stappen uitvoeren totdat er eentje fout gaat of flows die uitvoeren tot er eentje goed gaat of hij voert ze gewoon allemaal uit, serieel of parallel.  

   De programmataak beschrijft het programma, hoe het gestart wordt, de commandline, met welke parameters en welke omgevingsvariabelen. At runtime volstaat de commandline eventueel aangevuld met de inhoud van een directory tree. De directorytree wordt dan opgebouwd voordat het programma op basis van de commandline wordt opgestart. Het garandeert dat alles te starten is, van scripts, gecompileerde programma's met een framework tot standalone executables met eventuele shared libraries.  

   Het zijn dus de services die continue draaien en op de goede momenten, als alle resources aanwezig zijn en de tijd rijp is, hun proces starten. Nadat het proces gedraaid heeft, moet de start voorwaarde wel opheven worden. Meestal gebeurt dat in een archiverings stap nadat het proces goed geëindigd is. De services voorkomen fouten en hebben een zelfcorrigerend karakter. Het proces doet zijn ding en geeft aan of het goed of fout is gegaan, en de service regelt de foutafhandeling, het eventuele opnieuw proberen. De services kunnen reageren op events, zoals timer events, watch events op het file systeem op queue's etc.... 
   Het verwerken van data op de plek waar het afgeleverd wordt wil nog wel eens leiden tot het archiveren van nog niet verwerkte gegevens. De service kan hier een rol spelen door de te verwerken gegevens eerst naar de plaats van verwerking te brengen en dan het proces te starten.
   De service zorgt er ook voor dat de events serieel worden afgehandeld waardoor het proces niet perongeluk parallel wordt gestart. Gewenst parallel verwerken van gegevens is echter wel mogelijk.  
   Dit service niveau is belangrijk en mist nog al eens in oude systemen.  

   Het IT systeem draait op een Operating System, Windows of een Unix variant en start het programma "RunITDomein <domeinbeschrijvingvanhetdomein>". Het image van het Operating System wordt beschreven met een dockerfile.
 
#### **_De transformatie van bron beschrijving naar export file_**

 De beschrijving gaat dus volgens sterk getypeerde datastructuren. Door die formele beschrijving krijg je hulp van de Ide met intellisense en met behulp van de compiler zorgt de Ide ook voor fout detectie. De beschrijving wordt gecompileerd en uitgevoerd. Het resultaat is een versleutelde exportfile. Dat is de file die door "RunITDomein" wordt uitgevoerd. 
 
 De informatie die je oplevert gebeurt op basis van een parameter, de omgeving. Jij hebt de mogelijkheid iets anders op te leveren in de Ontwikkel omgeving dan in de Productie omgeving. Daarom vul je niet een datastructuur met een vaste waarde, maar maak je een functie die de waarde terug geeft op basis van zijn parameter. Vanwege dit mechanisme kan je ook eigen controles uitvoeren met als ultieme controle een praktijk test. Jouw IT domein wordt gecompileerd met gedeeltelijk eigen controles en eigen generatie logica.  

 De informatie haal je uit de gegevens van het bestaande systeem. Soms is die informatie nog niet geschikt om op te nemen: een Java of C# sourcefile is niet uitvoerbaar die moet eerst gecompileerd worden. Dat compileren kan je doen op het moment dat jouw functie wordt aangeroepen om de informatie aan te leveren voor de service waarin deze programmataak een rol speelt.

 Echt nieuwe dingen ga je niet doen. Een complete beschrijving maken, is verzamelen van wat er al is. En wat je tijdens het ontwikkel proces met de hand deed, ga je nu geautomatiseerd doen. Daarmee leg je de kennis in de hoofden van mensen vast in Git.   

### De datastructuren van een IT domein op een rijtje

 - Stap   
   een stap is een flow of een task
   een stap geeft altijd een resultaat
 - Flow  
   Een flow is een lijst van stappen
 - Taak
   Een taak is een programma of script of dll-aanroep 
   het is de bouwsteen van een proces
 - Proces  
   Een proces is een stap die op basis van een "voorwaarde" stap wordt uitgevoerd
 - Voorwaarde  
   Dit is een Stap die bepaald of een Proces uitgevoerd kan worden.
 - Service 
   Een Service verzorgt de uitvoering van een Proces op basis van de "voorwaarde" stap  
   De Service doet dit repeterend met een op te geven interval.
 - De server  
   De server wordt beschreven met een dockerfile  
   In die dockerfile staat het Operating System en alle noodzakelijke software beschreven.  
   Het entrypoint is het programma runITdomein met de domeinbeschrijving als parameter.

### Beheer van aanpassingen

Er wordt een change(aanpassing) opgenomen in Git als er iets veranderd moet worden in het domein. Van iedere service die het raakt, wordt een nieuwe versie gemaakt en gekoppeld aan de Change. De change doorloopt een levensloop van Ontwikkeling ... Productie. Door de koppeling met de versie weet de export functie in Git welke versies van de services in de beschrijving van de desbetreffende omgevingen komen.

