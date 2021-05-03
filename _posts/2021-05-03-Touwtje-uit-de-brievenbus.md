---
layout: post
title: "Nederland naïef, hoe halen we het touwtje uit de brievenbus van onze IT"
date: 2021-05-03
---

## (Te)Goed van vertrouwen.

Dat is eigenlijk een gunstige eigenschap, kennelijk vertrouwen wij elkaar en zijn we verbaasd maar ook boos als iemand dat vertrouwen schaadt. Maar te goed van vertrouwen is een vorm van naïviteit die genadeloos kan worden afgestraft, sprookjes zijn soms wreed maar je kan er wat van leren.

>Wie is daar? Ik ben het Grootmoe, Roodkapje! Ik breng U appels en wijn; doe maar vlug open!  "Trek maar aan het touwtje, dan zal de deur wel open gaan!" antwoordde Grootmoe. Nu sprong de deur vanzelf open...

De stafchef van Navalny betichtte onze parlementariërs van naïviteit, zijn zij sukkels? Nee, wij zijn hier in Nederland gewoon niet gewend aan zulke brutaliteit maar we worden er helaas mee geconfronteerd. Zie het als een wijze les, dat is ook de moraal van het sprookje. En trap er een tweede keer niet in.

## De goede oude tijd toen alles beter leek...

  Ja bij de laatste reünie van mijn oude school viel me op dat de gangen er krapper uitzagen, rijen kluisjes, dat was vroeger niet zo. Wij gooiden onze pukkel op de grond in de gang en niemand raakte wat kwijt. 

  Veel van onze IT systemen zijn gebouwd in die onbezorgde tijd, en veel daarvan zijn nog actief. Misschien is het slot wel verbeterd maar de sleutel wordt nog steeds aan Jan en alleman gegeven om noodzakelijk onderhoud ter plekke te doen. Misschien dat de sleutel beperkt houdbaar is en dat de ontvanger daarvan 3 handtekeningen moet zetten maar nog steeds lopen daar vreemden rond, die van alles kunnen zien en doen. De meesten zullen eerlijk zijn maar misschien wel slordig en iedereen kan gechanteerd worden, en infiltratie door de onderwereld is ook niet ondenkbeeldig.

  Het zijn misschien herkenbare zorgen: mijn dochter stak haar fietssleutel met daaraan een zware sleutelbos met onze huissleutel daarbij, in het slot van haar fiets. Het is gebeurd dat na een ritje alleen de fietssleutel over was. 

  Dacht je vroeger nog: wat moet iemand met mijn gegevens? Nu ben je als de dood dat jouw gegevens te koop worden aangeboden en dat je op de koop toe nog een boete krijgt omdat je nalatig bent geweest.

## Structureel oplossen, het fabrieksmodel.
  
  Door de sloten te verbeteren, en een goede sleutel administratie te voeren ben je nog niet veilig. Liever geen onderhoud ter plekke, geen vreemden over de (werk)vloer. Stel je maar een fabriek voor, in de hal is het drukte van je welste, toeleveranciers brengen de grondstoffen en vervoerders halen de eindproducten op. Verderop sleutelt iemand aan de machine en daar werkt (gedeeltelijk) gespecialiseerd personeel aan de productie op basis van een redelijk recent handboek. Er is een discussie gaande of de laatste revisie van de machine niet terug gedraaid was vanwege een probleem en dat daarom de oude instructies weer gelden. Er is iemand die het allemaal haarfijn weet maar die heeft vandaag een begrafenis en kan daarom eigenlijk niet gestoord worden. 

  **_Fabriek 1.0_**: Chaos door veel mensen en een dynamische omgeving waar veel veranderd. De productie medewerker krijgt instructies van de hem onbekende specialist die iets repareert aan de machine maar voelt zich daar niet prettig bij, door eigen ervaring lijkt die instructie niet te kloppen. Dit is een voedingsbodem voor problemen, en is een ideaal klimaat om ongemerkt diefstal of sabotage te plegen. Discussies zijn heel nuttig maar moeten plaats vinden bij het productontwikkeling en tijdens het testen of in een evaluatie fase maar liever niet tijdens productie. Ook het gedoe rond een klant die zijn producten kwam ophalen en uitvoerig de grondstoffen bestudeerde die in de hal stonden was niet in de goede aarde gevallen, het productie proces is het intellectueel eigendom van het bedrijf en dat wil je niet aan de grote klok hangen. Dus is het tijd voor Fabriek 2.0.

> **_Fabriek 2.0_**: Gesloten fabriekshal met gekoppelde kluizen voor toevoer van grondstoffen en afvoer van producten.

In de gesloten fabriekshal staat een grote machine. Aan de buitenkant zijn kluizen waar toeleveranciers met hun sleutel grondstoffen kunnen deponeren, het kan er alleen in maar niet uit. Aan de fabriekszijde kan de machine de grondstoffen uit de kluizen halen en via een magisch proces worden de eindproducten gemaakt die door de machine in afhaalkluizen worden gezet. Deze afhaalkluizen zijn aan de buitenkant van de fabriek te openen door vervoerders met een speciale sleutel die de producten vanuit de kluis in de auto kunnen zetten.

> In het laboratorium wordt gewerkt aan een nieuwe versie van de machine.

Er is ook nog een laboratorium  waar de machine wordt aangepast voor nieuwe of gewijzigde producten, daar werken specialisten aan, zij testen de nieuwe machine totdat die goed bevonden is. Tot die tijd staan er op laboratorium 2 machines, de oude en de nieuwe. De oude gaat daar pas weg als er geen enkele oude machine in bedrijf is. Soms wordt de machine in een fabriekshal vervangen door een nieuwe, en soms wordt een nieuwe hal gebouwd met een nieuwe machine eventueel gekoppeld aan de oude kluizen of aan nieuw gemaakte kluizen, die dan de oude fabriekshal vervangt.

## Wat los je ermee op?

  De fabriek wil constante kwaliteit op alle productie locaties, daarom wil men op locatie een machine die ontwikkeld is in het laboratorium, daar helemaal is getest en waarvan een exacte kopie op locatie wordt gezet. De machine is niet open te maken, is er wat aan de hand dan krijgt de locatie een nieuw gereviseerde versie uit het laboratorium. Er wordt niet gerommeld op locatie. 
  
  De grondstoffen zijn natuurlijk van belang voor de kwaliteit, de toeleverancier is zorgvuldig gekozen, de kluis is zo gemaakt dat er van de buitenkant alleen iets ingezet kan worden (alleen deponeren) en er kan alleen vanaf de machine iets uitgehaald worden. De machine controleert de kwaliteit voordat het de grondstof gebruikt.

  Het eindproduct wordt na controle van de machine, in een kluis voor de afnemer gezet. ook hier is het eenrichtingsverkeer, de machine zet het erin en de vervoerder haalt het namens de afnemer middels zijn sleutel, uit de kluis.

  De kluizen kunnen snel geblokkeerd worden. De machine mag er altijd bij maar de toeleverancier en afnemer kunnen bij onraad niet meer bij de kluis.

## De relatie tot een IT systeem

  Het laboratorium is het source control systeem, Git dus, waar de specialisten van het "DevOps team" de services bouwen en onderhouden die gezamenlijk de machine vormen. Zij zijn ook verantwoordelijk voor het bijhouden van de versies van de services en het uitrollen naar locatie. 

  Behalve het eenmalig neerzetten van de machine, de services in dit geval, is er verder geen behoefte om nog op locatie te komen, de interactieve shell, een heel krachtig maar gevaarlijk hulpmiddel, kan verwijderd worden. Onderhoud, beheer dus, zit in Git, er wordt verder niks van waarde op locatie bewaard, dat zit in de kluizen en databases en die heb je niet op de server staan. Zie ze voor je als een (cloud)dienst, het maakt niet uit waar ze staan als je ze maar kan bereiken. 

  Op deze manier kan de server ook eenmalig opgebouwd worden. Als er iets in de software verandert, gooi je de oude server weg en bouw je een nieuwe op basis van de gewijzigde software. Bij het wisselen van server moet er wel duidelijkheid zijn wie het laatse onderhanden product gaat (af)maken.
  
  Het is een risico om software die naar netwerkpoorten luistert te combineren met de software die je verwerking doet. De fabriekshal is helemaal afgesloten, en dat moet de server ook zijn. 
  
  Transport van gegevens gaat natuurlijk over het netwerk, dat besteed je uit aan een (externe) leverancier, gebruik bijvoorbeeld een Azure Storage Account , jij maakt die aan en je geeft een schrijf sleutel van een container aan de leverancier van input gegevens. Alles wat die daar schrijft, kan jij ophalen en verwerken. Je kan genotificeerd worden zonder dat jij als server op een netwerk poort luistert. Jouw storage leverancier zorgt voor alle tools, beveiliging en versleuteling en documentatie daarvan. Dat is handig en ook veilig. Je hoeft niet prijs te geven wat voor systeem je hebt, want de systemen wisselen nooit direct gegevens uit maar altijd via de storage leverancier.

  De log bevat maar al te vaak gegevens die je niet graag openbaar wil hebben. Het is belangrijke informatie dus sla je die op in een datakluis, bijvoorbeeld bij een storage leverancier. Lezen van de log vanaf de server moet niet mogelijk zijn.

  Het productieproces, die machine die alles doet, draait in het IT systeem alle processen als services die periodiek controleren of alle seinen op groen staan (gegevens aanwezig en het tijdstip in orde) en dan hun werk doen, en dat telkens weer. De machine bestaat uit 2 delen, een **_uitvoer(execute)_** gedeelte gevoed door de versleutelde **_instructies_**. De instructies zijn beschermd tegen lezen en aanpassen. Je zult toegang tot Git moeten hebben en de gebruikelijke weg volgen om hier wat aan te passen en die aanpassing actief te krijgen. Vanaf de server is het niet mogelijk.



## Is het zo makkelijk?

  Er is wel inspanning voor nodig, er is een goede beschrijving nodig waarmee de machine automatisch kan produceren, je moet versies beheren, goed kunnen testen om aan te tonen dat de machine werkt, je moet de geheimen bewaken die de machine nodig heeft om kluizen en databases en andere beschermde resources te benaderen.
### formele beschrijving

  Het werk voor een volledig automatisch werkend systeem, betekent volledig specificeren van alles wat nodig is om dat voor elkaar te krijgen. De specificatie moet je formeel maken, typeren of er een schema van maken. Exporteren en weer kunnen importeren, en zorgen voor achterwaartse compatibiliteit.

### Een runtime systeem.

  Er moet dus nog iets zijn wat op basis van een geïmporteerde versleutelde beschrijving een werkende machine maakt. Het draait als een tussenlaag waardoor het bijvoorbeeld sources van scripts vorborgen kan houden. Het voorkomt ongewenst parallel draaien (bron van veel ellende) door al de (trigger) events op een interne queue te plaatsen, maar maakt gewenst parallel draaien mogelijk. Het gaat ook zorgen voor diensten die het mogelijk maken bestaande programma's en scripts zonder aanpassingen te laten werken met nieuwe technieken. 

### Versie beheer

  Op basis van een aanpassing, een _change_, ga je een nieuwe versie van de machine ontwikkelen, sommige onderdelen worden vernieuwd, andere verwijderd en nieuwe ontstaan, van al die geraakte onderdelen maak je een nieuwe versie en die koppel je aan de _change_. De _change_ doorloopt een levensloop met als start de ontwikkelfase en als eind de productiefase. Met deze wetenschap kan je automatisch machines samenstellen vanuit de juiste versies van de onderdelen, voor iedere fase. 

### Bestaande programma's aanpassen aan de gewijzigde omstandigheden.

  De programma's zijn niet gewend om gegevens uit kluizen te halen of ze daar in te zetten. Je wilt natuurlijk het liefst geen aanpassingen aan bestaande programmatuur. 

  Soms kan je daarvoor de streams _stdin_ en _stdout_ gebruiken. De runtime kan het (ontcijferde)bestand dan streamen. Dat kan niet in alle gevallen, dan zou je vooraf het bestand op het filesysteem kunnen zetten en het achteraf kunnen verwijderen, met het gevaar dat iemand net de tijd heeft om iets van de inhoud van het bestand te zien.

  Als bestaande programma's geheimen in de source hebben staan, dan moet je die wel vanuit een kluis ophalen, die benadering hoeft niet in het bestaande programma te worden gedaan, het runtime systeem zou de waarde via de environment kunnen doorgeven.

  Testen is belangrijk, voordat een _change_ een fase verder gaat moet de hele machine goed gekeurd zijn. Soms kost het wat werk om dat geregeld te krijgen, maar het is natuurlijk altijd een noodzakelijke actie. 

## Werk te doen.

  Naar mijn beste weten is er werk te doen. Er zijn vast en zeker nog veel systemen 1.0. Wat wij horen in het nieuws is waarschijnlijk een topje van de ijsberg want ieder slachtoffer probeert het natuurlijk stil te houden.

  Met mijn uitgewerkte methodiek, gericht op migratie en nieuwbouw, ben [ik](info@paedelman.net) er klaar voor. Wie durft?

