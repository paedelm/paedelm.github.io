---
layout: post
title: "Nederland naïef, hoe halen we het touwtje uit de brievenbus van onze IT"
date: 2021-05-03
---

## (Te)Goed van vertrouwen.

Dat is eigenlijk een gunstige eigenschap, kennelijk vertrouwen wij elkaar en zijn we verbaasd maar ook boos als iemand dat vertrouwen schaadt. Maar te goed van vertrouwen is een vorm van naïviteit die genadeloos kan worden afgestraft, sprookjes zijn soms wreed maar je kan er wat van leren.

>Wie is daar? Ik ben het Grootmoe, Roodkapje! Ik breng U appels en wijn; doe maar vlug open!  "Trek maar aan het touwtje, dan zal de deur wel open gaan!" antwoordde Grootmoe. Nu sprong de deur vanzelf open...

De stafchef van Navalny betichtte onze parlementariërs van naïviteit, zijn zij sukkels? Nee, wij zijn hier in Nederland gewoon niet gewend aan zulke brutaliteit maar we worden er helaas mee geconfronteerd. Zie het als een wijze les, dat is ook de moraal van het sprookje. En trap er een tweede keer niet in.

## Vroeger...

  Ja bij de laatste reünie van mijn oude school viel me op dat de gangen er krapper uitzagen, rijen kluisjes, dat was vroeger niet zo. Wij gooiden onze pukkel op de grond in de gang en niemand raakte wat kwijt. 

  Veel van onze IT systemen zijn gebouwd in die onbezorgde tijd, en veel daarvan zijn nog actief. Misschien is het slot wel verbeterd maar het beheer gebeurt ter plekke waardoor Jan en alleman een sleutel nodig heeft. Binnen is echter niks meer afgeschermd, hier gaat alles op vertrouwen. Laat niemand dus zijn sleutel verliezen.
  
  Dacht je vroeger nog: wat moet iemand met mijn gegevens? Nu ben je als de dood dat jouw gegevens te koop worden aangeboden en dat je op de koop toe nog een boete krijgt vanwege nalatigheid.

### Betrouwbaarheid in relatie tot veiligheid
  
  De vroegere systemen zijn meestal niet ontworpen om volledig automatisch te werken. Als er wat mis ging dan had je altijd iemand die met de hand een correctie kon doen en het systeem daarna weer op kon starten.
  Tegenwoordig zien we het veiligheids risico hiervan, om te corrigeren moeten mensen toegang hebben tot het IT systeem, en als ze daar binnen zijn, hebben ze vaak (te) ruime bevoegdheden en is het soms moeilijk te zien wat er precies gedaan is. Dat kan voor iemand die naar beste eer en geweten werkt enorme stress opleveren omdat ieder foutje grote gevolgen kan hebben. De gevolgen die het heeft als iemand met een duister doel toegang heeft gekregen zien we regelmatig in het nieuws.

  Met zo'n corrigerende menselijke hand is de druk om het systeem van te voren heel goed te testen minder groot. 

### Beveiligen van gegevens

  Aan versleutelen van gegevens werd vroeger nauwelijks gedacht. Het was een gedoe en het kostte ook performance, zo werd gedacht. Je mag blij zijn als het transport versleuteld is.
  
  Nu complete gegevens bestanden verkocht worden op het dark net, is de overtuiging om te versleutelen wel doorgedrongen. 
  
  Nog steeds geldt dat een versleutelde opslag de meeste hoofdbrekens kost omdat aanpassen van het bestaande systeem een flink werk lijkt te zijn. Toch kan dat meevallen. Transport versleutelen is in veel gevallen makkelijker aan te passen. Het kan wat lastiger zijn als de verwerking het transport aanstuurt.

### Zorgvuldig omgaan met geheimen

  De oude systemen willen nog wel eens slordig omgaan met geheimen zoals toegangs gegevens tot databases en andere belangrijke beveiligde plekken. Daar moet je iets aan doen.


## Het fabrieks model
  
  
  Stel je maar een **_oude fabriek__** voor, in de hal is het enorm druk, toeleveranciers brengen de grondstoffen en vervoerders halen de eindproducten op. Verderop sleutelt iemand aan de machine en daar werkt (gedeeltelijk) gespecialiseerd personeel aan de productie op basis van een redelijk recent handboek. Er is een discussie gaande of de laatste revisie van de machine ongedaan gemaakt is vanwege een probleem en dat daarom de oude instructies weer gelden. Er is iemand die het allemaal haarfijn weet maar die heeft vandaag een begrafenis en kan eigenlijk niet gestoord worden. 


  De **_nieuwe fabriek_** ziet er anders uit: een gesloten fabriekshal met gekoppelde kluizen voor toevoer van grondstoffen en afvoer van producten.  
  In de gesloten fabriekshal staat een grote machine. Aan de buitenkant zijn kluizen waar toeleveranciers met hun sleutel grondstoffen kunnen deponeren, het kan er alleen in maar niet uit. Aan de fabriekszijde kan de machine de grondstoffen uit de kluizen halen en via een magisch proces worden de eindproducten gemaakt die door de machine in afhaalkluizen worden gezet. Deze afhaalkluizen zijn aan de buitenkant van de fabriek te openen door vervoerders met een speciale sleutel die de producten vanuit de kluis in de auto kunnen zetten.

  Naast de fabriek is er nog een laboratorium waar de machine wordt aangepast voor nieuwe of gewijzigde producten, daar werken specialisten aan, zij testen de nieuwe machine totdat die goed bevonden is. Dan vervangt hij de oude machine in de fabriekshal. 

### Wat zijn de verbeteringen?

  De fabriek wil constante kwaliteit op alle productie locaties, door het ontwikkelen en testen van de machine in het laboratorium en vandaar uit een complete installatie op locatie verzekert men zich van voorspelbaar gedrag. Mede door de grondstoffen te beschermen tegen diefstal en sabotage kan het eindproduct van de gewenste kwaliteit zijn. Door het eindproduct te beschermen tegen diefstal en sabotage krijgt de klant uiteindelijk zijn product. 
## De relatie tot een IT systeem

  Het laboratorium is het source control systeem, Git dus, waar de specialisten van het "DevOps team" de services bouwen en onderhouden en die gezamenlijk de machine vormen. Zij zijn ook verantwoordelijk voor het bijhouden van de versies van de services en het uitrollen naar locatie. Iedere aanpassing zou hier moeten gebeuren, en is daarom traceerbaar. De enige uitzondering hierop is het bijhouden van de kluis met geheimen.

  Behalve het eenmalig neerzetten van de machine, de services in dit geval, is er verder geen behoefte om nog op locatie te komen, de interactieve shell, een heel krachtig maar gevaarlijk hulpmiddel, kan verwijderd worden. Onderhoud, beheer dus, zit in Git, er wordt verder niks van waarde op locatie bewaard, dat zit in de kluizen en databases en die heb je niet op de server staan. 

  De kluizen van de fabriekshal worden (cloud)storage containers, ze voldoen aan alle veiligheids eisen, jij maakt die aan voor een leverancier en geeft die een schrijfsleutel. Alles wat daar geschreven wordt, kan jij ophalen en verwerken.  De hulpmiddelen en documentatie komen van jouw storage provider. Omdat het systeem van de storage provider er tussen zit, ben je losgekoppeld van jouw toeleverancier.

  De log bevat maar al te vaak gegevens die je niet openbaar wil hebben. Het is belangrijke informatie dus sla je die op in een datakluis. 

  Het productieproces, die machine die alles doet, draait in het IT systeem alle processen als services die periodiek controleren of alle seinen op groen staan (gegevens aanwezig en het tijdstip in orde) en dan hun werk doen, en dat telkens weer. De machine bestaat uit 2 delen, een **_uitvoer(execute)_** gedeelte gevoed door de versleutelde **_instructies_**. De instructies zijn beschermd tegen lezen en aanpassen. 



### Wat moet je ervoor doen?

### formele beschrijving
  De inspanning zit voornamelijk in het volledig beschrijven van het proces, met het resultaat van die beschrijving kan de machine autonoom zijn werk doen. De bron beschrijving bevat echter extra informatie, nodig voor het controleren, testen en versioneren. 

### Een runtime systeem.

  Dit is de machine, die werkt op basis van de geïmporteerde versleutelde beschrijving. Het verbergt bijvoorbeeld sources van scripts en voorkomt ongewenst parallel draaien (bron van veel ellende) maar maakt gewenst parallel draaien mogelijk. Het faciliteert diensten die het mogelijk maken bestaande programma's en scripts zonder aanpassingen te laten werken met nieuwe technieken. 

### Versie beheer

  Op basis van een aanpassing, een _change_, ga je een nieuwe versie van de machine ontwikkelen, sommige onderdelen worden vernieuwd, andere worden verwijderd en nieuwe ontstaan, van al die geraakte onderdelen maak je een nieuwe versie en die koppel je aan de _change_. De _change_ doorloopt een levensloop met als start de ontwikkelfase en als eind de productiefase. Deze kennis wordt gebruikt voor het samenstellen van de juiste versies voor iedere fase. 

### Bestaande programma's aanpassen aan de gewijzigde omstandigheden.

  De programma's zijn niet gewend om gegevens uit kluizen te halen of ze daar in te zetten. Je wilt natuurlijk het liefst geen aanpassingen aan bestaande programmatuur. 

  Soms kan je daarvoor de streams _stdin_ en _stdout_ gebruiken. De runtime kan het (ontcijferde)bestand dan streamen of met een extra stap vooraf op het filesysteem zetten.

  Verwijderen van geheimen uit alle sources, het runtime systeem zou de geheimen via de environment kunnen doorgeven.

  Testen is belangrijk, voordat een _change_ een fase verder gaat moet de hele machine goed gekeurd zijn.

## Werk te doen.

  Naar mijn beste weten is er werk te doen. Er zijn vast en zeker nog veel naïve systemen. Wat wij horen in het nieuws is waarschijnlijk een topje van de ijsberg want ieder slachtoffer probeert het waarschijnlijk stil te houden.

  Met mijn uitgewerkte methodiek, gericht op migratie en nieuwbouw, ben [ik](info@paedelman.net) er klaar voor. Wie durft?

