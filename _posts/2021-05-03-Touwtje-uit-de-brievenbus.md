---
layout: post
title: "Nederland naïef, hoe halen we het touwtje uit de brievenbus van onze IT"
date: 2021-05-03
---

## (Te)Goed van vertrouwen.

Dat is eigenlijk een gunstige eigenschap, kennelijk vertrouwen wij elkaar en zijn we verbaasd maar ook boos als iemand dat vertrouwen schaadt. Maar te goed van vertrouwen is een vorm van naïviteit die genadeloos kan worden afgestraft, sprookjes zijn soms wreed maar je kan er wat van leren.

>Wie is daar? Ik ben het Grootmoe, Roodkapje! Ik breng U appels en wijn; doe maar vlug open!  "Trek maar aan het touwtje, dan zal de deur wel open gaaan!" antwoordde Grootmoe. Nu sprong de deur vanzelf open.

De stafchef van Navalny betichtte onze parlementariërs van naïviteit, zijn zij sukkels? Nee wij zijn hier in Nederland gewoon niet gewend aan zulke brutaliteit maar we worden er helaas wel mee geconfronteerd, en negeren is geen optie, je ziet aan het sprookje hoe fout dat kan gaan. Door schade en schande word je wijs dus een tweede keer mag dat ze niet meer gebeuren.

## De goede oude tijd toen alles beter leek...

  Ja bij de laatste reünie van mijn oude school viel me op dat de gangen er krapper uitzagen, rijen kluisjes, dat was vroeger niet zo. Wij gooiden onze pukkel op de grond in de gang en niemand raakte wat kwijt. 

  Veel van onze IT systemen zijn gebouwd in die onbezorgde tijd, en veel daarvan zijn nog actief. Misschien is het slot wel verbeterd maar de sleutel wordt nog steeds aan Jan en Alleman gegeven om noodzakelijk onderhoud ter plekke te doen. Misschien dat de sleutel beperkt houdbaar is en dat de ontvanger daarvan 3 handtekeningen moet zetten maar nog steeds lopen daar vreemden rond, die van alles kunnen zien en doen. Gelukkig zijn de meesten eerlijk maar vanuit mijn banktijd weet ik dat eerlijke mensen ook gechanteerd kunnen worden, en van tegenwoordig is bekend, onlangs nog, dat de onderwereld infiltreert.

  Het is misschien herkenbaar, mijn dochter reed op haar fiets met aan de fietssleutel een zware sleutelbos (of andersom) met o.a. onze huissleutel. Zo kan je zorgen hebben over sleutels van je huis.

  Dacht je vroeger nog: wat moet iemand met die gegevens? Nu ben je als de dood dat jouw gegevens te koop worden aangeboden en dat je op de koop toe nog een boete krijgt omdat je nalatig bent geweest.

## Structureel oplossen, het fabrieksmodel.
  
  Dus door de sloten te verbeteren, en een goede sleutel administratie te voeren ben je nog niet veilig. |Eigenlijk wil je van dat onderhoud af in huis, geen vreemden over de vloer. Nu is een huis niet de beste vergelijking, omdat er ook gewoond wordt, een fabriek lijkt beter op een IT systeem.

  > Gesloten fabriekshal met gekoppelde kluizen voor toevoer van grondstoffen en afvoer van producten.

   In de gesloten fabriekshal staat een grote machine. Aan de buitenkant zijn kluizen waar toeleveranciers met hun sleutel grondstoffen kunnen deponeren, het kan er alleen in maar niet uit. Aan de fabriekszijde kan de machine de grondstoffen uit de kluizen halen en via een magisch proces worden de eindproducten gemaakt die door de machine in afhaalkluizen worden gezet. Deze afhaalkluizen zijn aan de buitenkant van de fabriek te openen door vervoerders met een speciale sleutel die de producten vanuit de kluis in de auto kunnen zetten.

 > In het labaratorium wordt gewerkt aan een nieuwe versie van de machine.

  Er is ook nog een laboratorium  waar de machine wordt aangepast voor nieuwe of gewijzigde producten, daar werken specialisten aan, zij testen de nieuwe machine totdat die goed bevonden is. Tot die tijd staan er op laboratorium 2 machines, de oude en de nieuwe. De oude gaat daar pas weg als er geen enkele oude machine in bedrijf is. Soms wordt de machine in een fabriekshal vervangen door een nieuwe, en soms wordt een nieuwe hal gebouwd met een nieuwe machine eventueel gekoppeld aan de oude kluizen of aan nieuw gemaakte kluizen, die dan de oude fabriekshal vervangt.

## Wat los je ermee op?

  De fabriek wilde constante kwaliteit op alle productie locaties, daarom wilde men op locatie een machine die ontwikkeld was in het laboratorium, daar helemaal getest, en waarvan een exacte kopie op locatie wordt gezet. De machine niet open te maken, is er wat mee, dan krijgt de locatie een nieuw gereviseerde versie uit het laboratorium. Er wordt niet gerommeld op locatie. 
  
  De grondstoffen zijn natuurlijk van belang voor de kwaliteit, de toeleverancier is zorgvuldig gekozen, de kluis is zo gemaakt dat er van de buitenkant alleen iets ingezet kan worden (write only) en er alleen vanaf de machine iets uitgehaald kan worden. De machine controleert de kwaliteit voordat het de grondstof gebruikt.

  Het eindproduct wordt na controle van de machine, in een kluis voor de afnemer gezet. ook hier is het eenrichtings verkeer, de machine zet het erin en de vervoerder haalt het namens de afnemer middels zijn sleutel, uit de kluis.

  De kluizen kunnen snel geblokkeerd worden. De machine mag er altijd bij maar de toeleverancier en afnemer kunnen bij onraad niet meer bij de kluis.

## De relatie tot een IT systeem

  Het laboratorium is het source control systeem, Git dus, waar de specialisten van het DevOps team de services bouwen en onderhouden die gezamenlijk de machine vormen. Zij zijn ook verantwoordelijk voor het bijhouden van de versies van de services en het uitrollen naar locatie. 

  Behalve het eenmalig neerzetten van de machine, de services in dit geval, is er verder geen behoefte om nog op locatie te komen. Onderhoud, beheer dus, zit in Git, er wordt verder niks van waarde op locatie bewaard, dat zit in de kluizen en die hoeven in tegenstelling tot de fabriek niet letterlijk vast te zitten aan de server. Zie ze voor je als een (cloud)dienst, het interessert niet waar ze staan als je ze maar kan bereiken. Alleen het onderhanden product zit nog in de machine, jouw machine moet wel de opdracht krijgen te stoppen met produceren.

  Als je er niet meer hoeft te zijn behalve bij het opbouwen, dan kan je er een zogenaamde _immutable_ server van maken, aanpassen kan niet, las de deuren en ramen maar dicht, de software hoeft nergens poorten open te hebben staan, de kluizen worden puur op eigen initiatief benaderd, en de leverancier van de kluizen (Coud storage leverancier) zorgt met zijn of haar beveiligings experts dat je gegevens daar en het transport er naar toe en vanaf, veilig zijn. 

## Is het zo makkelijk?
  Er is wel inspanning voor nodig, er is een goede beschrijving nodig waarmee de machine kan produceren, je moet versies beheren, goed kunnen testen om aan te tonen dat de machine werkt, je moet de geheimen bewaken die de machine nodig heeft om kluizen en databases en andere beschermde resources te benaderen.
  ### formele beschrijving

  Het werk voor een volledig automatisch werkend systeem betekent volledig specificeren van alles wat nodig is om dat voor elkaar te krijgen. De specificatie moet je formeel maken, typeren of er een schema van maken, en moet je kunnen exporteren en weer importeren om jouw machine daarmee werkend te krijgen.

  ### Een runtime systeem.

  Er moet dus nog iets zijn wat op basis van een geïmporteerde beschrijving een werkende machine maakt.

  ### Versie beheer

  Op basis van een aanpassing, een _change_, ga je een nieuwe versie van de machine ontwikkelen, sommige onderdelen worden vernieuwd, andere verwijderd en nieuwe ontstaan, van al die geraakte onderdelen maak je een nieuwe versie en die koppel je aan de _change_. De _change_ doorloopt een levensloop met als start de ontwikkelfase en als eind de productiefase. Met deze wetenschap kan je machines samenstellen vanuit de juiste versies van de onderdelen voor iedere fase. 

  ### Bestaande programma's aanpassen aan de gewijzigde omstandigheden.

  De programma's zijn niet gewend om gegevens uit kluizen te halen of ze daar in te zetten. Je wilt natuurlijk het liefst geen aanpassingen aan bestaande programmatuur. 

  Soms kan je daarvoor de streams _stdin_ en _stdout_ gebruiken. De runtime kan het (ontcijferde)bestand dan streamen. Dat kan niet in alle gevallen, dan zou je vooraf het bestand op het filesysteem kunnen zetten en het achteraf kunnen verwijderen, met het gevaar dat iemand net de tijd heeft om iets van de inhoud van het bestand te zien.

  Als bestaande programma's geheimen in de source hebben staan, dan moet je die wel vanuit een kluis ophalen, die benadering hoeft niet in het bestaande programma te worden gedaan, het runtime systeem zou de waarde via de environment kunnen doorgeven.

  Testen is belangrijk, voordat een _change_ een fase verder gaat moet de hele machine goed gekeurd zijn. Soms kost het wat werk om dat geregeld te krijgen, maar het is natuurlijk altijd een noodzakelijke actie. 


