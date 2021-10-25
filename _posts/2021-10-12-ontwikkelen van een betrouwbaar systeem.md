---
layout: post
title: "Ontwikkelen van een betrouwbaar en veilig IT-systeem"
date: 2021-10-12
---
>Een veilig systeem is beschermd tegen diefstal, sabotage en gijzeling. Om dat te bereiken neem je beveiligingsmaatregelen. Er zal echter geen maatregel zijn waar je volledig op kan vertrouwen, dus neem je er meerdere achter elkaar, in lagen. Pas na de laatste laag is het systeem verloren. Binnen de tijd die nodig is voor een aanvaller om daar te komen, zal jij het systeem al hebben vervangen door een nieuwe met gewijzigde sleutels. Bij het nieuwe systeem moet de aanvaller weer van voren af aan beginnen. Informatie uit het oude systeem, zou je er nog bij kunnen komen, is achterhaald dus onjuist en onbruikbaar. Omdat een aanvaller jou niet zal melden dat hij bezig is, vervang je het systeem altijd binnen de kritische tijd. Oudere systemen zijn niet zo ontworpen. Om ze veilig te maken is een verbeterde toegangscontrole alleen niet voldoende, je zal ze ook moeten herontwerpen. Een betrouwbaar systeem is op zijn minst veilig, het is goed getest en iedere wijziging is vanuit de bron traceerbaar.

## Een veilig systeem, hoe werkt dat?
Een veilig systeem om gegevens te verwerken lijkt op een tijdelijke werkplaats zonder vast adres. Voor het inrichten is een locatie met stroom en een netwerk aansluiting nodig, daar komt één machine in, eenmaal aangesloten wordt die gestart, de locatie wordt afgesloten en de sleutel (niet meer nodig) wordt weggegooid. De machine kan contact maken met de zwaar beveiligde externe datakluizen van een gerenommeerde beheerder. Alleen vanuit die kluizen verwerk je gegevens en het resultaat sla je daar ook weer op. Klanten die gegevens voor jou hebben, zetten dat in een kluis bij jouw beheerder, en gegevens die jij maakt voor een klant zet je in een daarvoor bedoelde kluis. De werkplaats staat dus op een geheime plek (laag 1), is beveiligd (laag 2), binnen in vind je een gesloten machine die toegang heeft tot de datakluizen maar die machine is beveiligd met een sleutel (laag 3). Iedere sleutel is met voldoende tijd te kraken. Daarom verander je binnen die tijd de sleutels van de datakluizen zodat deze machine waardeloos wordt (laag 4), intussen heb je een nieuwe machine met nieuwe sleutels in een nieuwe tijdelijke werkplaats gezet. De machine in de oude werkplaats weet dan dat zijn tijd voorbij is en de nieuwe neemt het werk over. Niemand behalve jouw databeheerder is zich bewust van deze verhuizing. Of misschien die inbreker wiens werk hierdoor ruw verstoord is en helemaal opnieuw moet beginnen.

## Een veilig systeem, hoe bouw je dat?
 Het bouwen van zo'n systeem, met die enkele machine is eigenlijk alleen te doen als je dat in hoge mate automatiseert. Gelukkig is dat ons vak. En ik heb me daar mee bezig gehouden. Kenmerkend is het in één keer plaatsen van een systeem en nooit meer aanpassen, in tegenstelling tot het plaatsen en stukje bij beetje aanpassen op locatie. Het beschermen van de gegevens kan je gelukkig uitbesteden. Die databeheerders, meestal Cloud leveranciers, zijn verschrikkelijk goed in beveiligen van de opslag en het transport van de gegevens, het enige wat jij moet doen is er gebruik van maken en niet proberen dat zelf te doen. Hou rekening met een (ongewenste) bezoeker op het systeem. Door jouw eigen software te verpakken en te versleutelen, voorkom je aanpassingen aan de software en bescherm je gevoelige informatie zoals sleutels tot belangrijke bronnen. Geef nooit impliciete rechten op die data bronnen, hoe makkelijk maak je het de hacker als hij via een fileshare jouw data kan gijzelen. Omdat je iets oplevert wat je niet meer kan aanpassen moet het natuurlijk compleet, gecontroleerd en goed getest zijn. Het systeem kan pas stabiel draaien als het ook onafhankelijk is van wijzigingen buiten zijn domein. Als laatste moet het systeem makkelijk te vervangen zijn, iedere maatregel die dat moeilijker maakt moet je kritisch bekijken want (snel) vervangen is een essentieel onderdeel (laag 4) in de verdediging tegen aanvallers.    
## Automatiseren van het ontwikkelen

 Ontwikkelen is een creatief proces en daarom misschien lastiger te automatiseren, toch valt er in de meeste IT systemen een duidelijk patroon te herkennen en kan je op basis daarvan automatiseren. Daarmee zorg je dat bekende problemen op dezelfde manier worden opgelost, er gebruik wordt gemaakt van gemeenschappelijke kennis en er niks vergeten wordt. Met het patroon als uitgangspunt, kan je templates ontwikkelen en middels een (OO)framework kan je doorbouwen op een bewezen fundament zonder redundantie te veroorzaken. Het zijn de technieken uit de lange historie van de software engineering die ons ook hierbij helpen. Bijkomend voordeel van deze automatisering is naast de veiligheid, het verminderen van menselijke inzet, dus besparen van kosten en een grote toename van de betrouwbaarheid van het systeem.

## Mijn patroon voor een IT achtergrond systeem

### De context
 Ik ga hier uit van systemen voor niet interactief gebruik dat betekent dat er geen HTTPS services worden aangeboden zoals een API of een Webserver, de processen kunnen wel API's afnemen dus zelf HTTPS client zijn, de API is dan één van de resources die waarschijnlijk beveiligd is met een API-sleutel. Als het domein interactieve diensten verleent dan doet dat het vanaf een ander systeem en laat het contact met de gebruiker via een professionele beheerder lopen, zoals een gateway van een Cloud leverancier. 

### Het domein proces
 Het IT systeem draait de processen van één domein. De processen benaderen de resources van het domein voor invoer, log en resultaat. Zo'n proces bestaat uit een "Verwerk" en "Archiveer" stap en vooraf de stap "GereedVoorVerwerking" die bepaalt of het proces gestart kan worden. Voor het testen heb je deze extra stappen: aan het begin "Test Initialisatie" en aan het einde "Test Afsluiting". Zo'n stap is een programma of een flow van stappen. Een stap heeft altijd een uitkomst van goed en fout, en een flow kent een uitvoeringsstrategie zoals bijvoorbeeld "draai tot de eerste stap die fout (of goed) gaat" en "draai alle stappen onconditioneel". Het resultaat van de flow is ook te configureren, zoals "het resultaat van de laatste stap", "eentje fout is fout" of "altijd goed", er zijn geen beperkingen. Script ontwikkelaars zal het bekend in de oren klinken. Er zijn ook mogelijkheden om stappen parallel te draaien, bijvoorbeeld een dynamische flow waarvan de stappen worden bepaald door een aantal files en waarvan de files (parallel) verwerkt worden. 

### Resources
 Dat kunnen bestanden zijn, berichten op een queue, een database en een Api. De processen zijn altijd klant van die resources en de toegangscontrole, indien aanwezig, is expliciet, meestal op basis van een (tijdelijke)sleutel en de rechten zijn afgepast op het gebruik.

### Een programma stap  

 In de definitie van de programma stap wordt beschreven hoe die gestart wordt, wat de parameters zijn, en wat er aan eventuele extra informatie via de environment of het filesysteem doorgegeven wordt. Ook is het mogelijk files te koppelen aan de standaard streams van het proces eventueel met versleuteling of ontcijfering. Er zijn geen beperkingen in welke taal of systeem het geschreven is maar de verwerking moet eindig zijn.

### De service
 Wanneer een proces wordt gestart en onder welke conditie, staat beschreven in de service-definitie. De service draait periodiek op basis van timer events eventueel gecombineerd met gegevens events en bepaalt op basis van de "GereedVoorVerwerking" stap of het proces gestart wordt.

### Levens cyclus
 Systemen veranderen, die veranderingen worden ontwikkeld en getest, zo ontstaan versies en test-omgevingen, de nieuwe versie wordt getest terwijl de oude nog actief is.
 <br/>
 De aangepaste en nieuwe services die nodig zijn krijgen hun eigen versienummer die gekoppeld wordt aan de "Wijziging" die zelf gekoppeld is aan een omgeving. De oude versie blijft in tact. De "Wijziging" wordt getest, en kan na akkoord gezet worden op de volgende omgeving, uiteindelijk wordt hij in productie genomen. Door de koppeling met de "Wijziging" weet het systeem welke versie van de service in welke omgeving thuis hoort. Oude versies kunnen opgeruimd worden als ze niet meer in productie actief zijn, en de nieuwe versie zich bewezen heeft.
 
### Afhankelijkheden op basis van de omgeving
 Er zijn verschillen per omgeving in de verwerking van de flows, dat kan divers zijn, de ontwikkelaar heeft de controle want die krijgt door voor welke omgeving hij de service definitie aanmaakt.
 
### Gevaren bij het gebruik van meerdere omgevingen
 Door de verschillende omgevingen kunnen lastig te ontdekken foutjes ontstaan, bijvoorbeeld het benaderen van een resource uit een andere omgeving, om dit te ontdekken krijg je de mogelijkheid dit te controleren en moet je dit accorderen.

### Opleveren van een container image
 Alle services bij elkaar draaien op een Operating System (OS), op dit OS moet de juiste software draaien, het patroon maakt gebruik van een Dockerfile waar met een ontwikkel image de software wordt gebouwd en getest om daarna gekopieerd te worden naar een kaal RUNTIME image.  Iedere omgeving krijgt zijn eigen image. Het image als geheel wordt gescand op bekende kwetsbaarheden.
  
 
## Hulpmiddelen voor het automatiseren van het ontwikkelen van een IT-systeem

  Voor het maken van een nieuw domein-systeem maak je een nieuwe Git-repository op basis van een template repository. De zo verkregen repository bevat een .NET template voor het maken van een nieuw domein-systeem. Is het domein aangemaakt dan gebruik je de .NET templates voor het maken van nieuwe (versies van) services. Het domein bevat ook een Dockerfile voor het maken van een image per omgeving.
  <br/><br/>
  Het Service template genereert source(s) in C#, op basis van OO schrijf je de functies die de definities van de verschillende programma stappen en de definities voor de scheduling van de service opleveren. De functies krijgen altijd de omgeving als parameter door en geven een optioneel resultaat. Zo kan je per omgeving variëren, en kan een service alleen bestaan voor een bepaalde omgeving. Een nieuwe versie van een service heeft standaard als basis de oudere versie van die service. Als je wil hoef je alleen de verschillen te programmeren.
  <br/><br/>
  Geheimen mogen niet zichtbaar zijn in de source. Er is een C# source waarin de namen (niet de waarden) van de geheimen worden bijgehouden. Ieder geheim, zoals een sleutel voor een resource, wordt toegevoegd aan een enumeratie in deze source, en met behulp daarvan opgezocht in een externe kluis. Het geheim kan vanuit de source van de service opgehaald worden met de enumeratie waarde (intellisense mogelijk). In deze source wordt gecontroleerd of de resource waarden wel corresponderen met de omgeving (op basis van naamgeving-conventie) en er is een controle of de resources wel toegang krijgen met deze waarden.
  <br/><br/>  
  Met .NET reflection in het "Develop.{domein}" programma worden de services verzameld, gecontroleerd en getest, eigen controles kunnen geprogrammeerd worden in de generatie functies, testen worden uitgevoerd met de "GereedVoorVerwerking" en "Verwerking" stappen op basis van de "Test Initialisatie" stap. Als de controles en testen goed gegaan zijn wordt er per omgeving een versleutelde JSON file gemaakt van de definities. Versleuteld omdat niemand de inhoud mag zien of aanpassen, er staan namelijk geheimen in. Deze configuratie wordt namelijk gebruikt in het bijbehorende RUNTIME systeem wat alleen deze configuratie kan lezen.
  <br/><br/>
  Docker wordt gebruikt om vanaf het development-image het kale RUNTIME-image te bouwen. Daar komt in ieder geval het "Run.{domein}" programma te staan met zijn bijbehorende versleutelde JSON-file. Per omgeving wordt er een container-image gemaakt. Scannen van het image op bekende kwetsbaarheden is de laatste controle.  
  <br/><br/>
  Alle hulpmiddelen worden actief onderhouden, zijn heel bekend, vrij te gebruiken en hebben een rijke historie. Bijna niks is onmogelijk, er is een enorme ondersteuning in de markt, en door de mogelijkheid van typesafe ontwikkelen, goede ontwikkelomgevingen, eenvoudig controles inbouwen en testen uitvoeren, verklein je de kans op fouten enorm. 

## Waar kan ik bij helpen?
### Inzet als vraagbaak en sparringspartner
  Meedenken en brainstormen over diverse onderwerpen zoals "veilig maken" van systemen of een deel onderwerp zoals het automatiseren van het ontwikkelen, gebruik maken van moderne features van .NET, Docker en VSCode, opdelen van processen in herkenbare stappen, streamen van (versleutelde) files, gebruik maken van queues voor triggering of domein data, publiceren van dashboard gegevens ten behoeve van monitoring, opvangen van STDIN, STDOUT en STDERR van script processen ten behoeve van (versleutelde)log of bestanden, gebruik van sleutel kluizen, voorkomen van onveilige mounts van file resources, hoe je met behulp van de api (recursieve)data structuren kan (de)serializeren en daarmee zelfs BLOBS in JSON verpakt en daarmee ook de inhoud van een directory tree, hoe je asynchroon op een event van een AZURE storage account kan wachten met alleen een HTTPS verbinding, hoe je het voor elkaar krijgt om alles traceerbaar via git te doen, hoe je met Fire&Forget een Request met uitgestelde Response kan doen en hoe je de uitvoering daarvan controleert, hoe je zo handig mogelijk transformaties programmeert, wat je allemaal niet met reguliere expressies kan oplossen, dat je alle scriptjes, programma's bij elkaar in één repository kan stoppen, hoe je templates gebruikt, OO inzet, pattern matching kan doen met de nieuwe C# select expression, platform onafhankelijke code schrijft, scripts intern kan uitvoeren waardoor ze niet leesbaar en wijzigbaar zijn en nog veel meer, ik heb een enorme ervaring, dus daarmee veel kennis uit het verleden maar ook een goed inzicht in de mogelijkheden van nu.

### Gebruik maken van templates
  Je kunt met mijn hulp gebruik maken van mijn templates, ongewijzigd of aangepast aan jouw omstandigheden. 

### Opzetten en gebruiken van veilige resources
  
  Gebruik maken van mijn ervaring te werken vanuit veilige resources, hoe je gegevens tijdelijk op de plek van verwerking krijgt en resultaten weer in zo'n resource opslaat waarbij geen gevoelige gegevens leesbaar achterblijven op het systeem van verwerking. Vooral bij files is het misschien wennen, het storage systeem als fileshare gebruiken is onveilig want dan is het toegankelijk voor ieder die gebruik maakt van de user waarmee gemount is. Dus moet je vanuit het programma met de toegangssleutel, via de Api het werk doen. Aandachtspunten zijn onder andere veilige toegang, koppelen aan streams, tijdelijk (versleuteld) opslaan, hoe je notie krijgt van events op een resource, wat je moet doen om gegevens niet dubbel te verwerken of vergeet te verwerken en hoe je kan bemerken dat nieuwe gegevens niet (op tijd) verwerkt worden. Het patroon bied je allerlei configureerbare mogelijkheden maar ook de mogelijkheid het op eigen wijze te doen.

### Monitoring
  Hoe je gebruik kan maken van een generieke monitoring op basis van mijn patroon. Het is **niet** nodig om domein specifieke logs (met eventueel gevoelige informatie) te gebruiken om het domein in de gaten te houden. Door de indeling in generieke stappen en de "goed/fout" resultaten is een generiek dashboard mogelijk. In de gaten wordt gehouden of er nog steeds gekeken wordt of het proces gestart kan worden, en als dat waar is of het proces dan op tijd gestart wordt en binnen redelijke tijd tot een goed eind komt. Wat op tijd is, en wat vaak genoeg is, geef je aan in de service definitie. De dashboard informatie wordt door het RUNTIME systeem gepubliceerd, en het dashboard zelf is een client applicatie die een abonnement heeft op de publicatie. De publicatie is natuurlijk beveiligd. 

### Scripts splitsen in functionele eenheden  
  Ik kan helpen bij het omvormen van scripts en programma's zodat ze passen binnen het patroon. Denk aan een script wat een combinatie is van "is verwerking al mogelijk" en de verwerking zelf, het patroon zal niet schrikken dat de verwerking nog niet mogelijk is als het ieder uur kijkt bij een proces wat dagelijks hoort te draaien. Verdere aandachtspunten zijn de foutcodes en ingebouwde parallelle verwerking om die laatste eventueel om te zetten naar de veilige parallelliteit van het patroon.

### Externe resources gebruiken voor communicatie met partners
  Directe communicatie is met jouw anonieme systeem niet mogelijk. Als jouw partner gegevens in files naar jou stuurt om te verwerken, kan je bijvoorbeeld een storage account in Azure aanmaken en de partner daar een (tijdelijke) schrijfsleutel van geven. De partner kan de Azure documentatie gebruiken om de file te uploaden, dat moet de partner wel lukken want transport is op heel veel manieren te doen. Transport en opslag zijn uiteraard versleuteld en jij kan de gegevens er weghalen eventueel op basis van een trigger en verwerken. Dat scheelt ook de kosten en moeiten van een exotisch file transfer tool.

### Praktische tips bij uitvoering
  Helpen bij allerhande praktische zaken zoals het handig gebruiken van "intellisense" voor niet alleen C# programma's maar voor alles wat je gebruikt (Python, NodeJs, Java, Shell script...) zelfs als je de source genereert vanuit C#. Hoe je vanuit jouw functie waarbij je de service definitie aanmaakt, controles kan doen, kan compileren, genereren etc. Dat hoeft niet speciaal in C#, je kan daarvoor gebruiken wat je wil.

### Documentatie
  Documentatie is nooit de hobby van ontwikkelaars, gelukkig kan je een patroon veel makkelijker geautomatiseerd documenteren. Soms is er dan wel wat korte informatie nodig voor een service of een stap. Alhoewel niet noodzakelijk voor het draaien is die informatie heel nuttig. Met het patroon kan je de gebruiker daarvan verplichten die informatie in te vullen. Zo kan je een Domein definitie aanmaken die uitstekend geschikt is voor het aanmaken van documentatie. Ik heb daarvoor al een "Blazor" project aangemaakt die een website genereerd van jouw domein definitie. Up to date documentatie!

### Log informatie verzamelen en bundelen
  Een algemene log om vast te leggen wat er gebeurd is in het domein kan gevoelige informatie bevatten en behoort daarom ook niet thuis op het systeem wat de verwerking van het domein doet. Het RUNTIME systeem kan alle log informatie van alle proces stappen opvangen als die via een standaard stream worden geschreven. Deze informatie kan dan naar een (versleutelde) beveiligde resource geschreven worden die bedoeld is voor loggen. Als er (gevoelige en niet versleutelde) log info naar het lokale filesysteem wordt geschreven dan zou dat aangepast moeten worden. Ik kan helpen bij het aanpassen van het loggen, en waar loggen wordt gebruikt voor monitoren, zorgen dat het standaard patroon daarvoor gebruikt wordt. 


## Vragen!
### Over ontwikkelen
#### Q: Is het nu nodig om alle processen in C# te ontwikkelen?
#### A: Nee, Je kan voor iedere programma stap in de procesflow een andere taal gebruiken. C# wordt gebruikt om de definitie van het domein op te stellen.
#### Q: Hoe laat je scripts draaien op bestanden in een beveiligd Cloud storage systeem?
#### A: Je kan de programma stap zo configureren dat een bestand uit de Cloud wordt gekoppeld aan de standard input stream van het proces. Je kan het bestand ook tijdelijk lokaal neerzetten eventueel versleuteld met een tijdelijke sleutel die je doorkrijgt als parameter of via de environment. Het is vanwege veiligheid **niet** aan te raden het storage systeem te mounten op het lokale filesysteem.
#### Q: Hoe krijg je een trigger voor een event op de een beveiligde resource in de Cloud?
#### A: De meest gebruikelijke manier is het event te publiceren op een queue. Deze queue wordt gekoppeld aan de service. Dit kan je gewoon configureren en hoef je niet te programmeren.
#### Q: Hoe werken events, wordt het proces direct aangeroepen?
#### A: Iedere service heeft een eigen interne queue. Ieder event, waar vandaan dan ook, wordt op die queue gezet. De service draait een enkele loop: wacht op een bericht van de interne queue, ga verwerken. Het is simpel, je draait nooit iets tegelijkertijd en bij een groot aanbod raakt niks in de stress.
#### Q: Waarom heb je C# nodig, je kan toch ook direct JSON of YAML definiëren?
#### A: Met een getypeerde programmeertaal maak je minder snel fouten, kan je eenvoudiger gebruik maken van gedeelde logica, en kan je eenvoudiger op basis van conditie werken. JSON is een exportformaat, en ingewikkelde JSON zonder fouten typen is geen sinecure.
#### Q: Hoe krijg ik een ingewikkelde programma stap met vereiste (binaire) configuratie bestanden geconfigureerd?
#### A: In de functie waarin de servicedefinitie wordt gemaakt bouw je een directory(tree) op met alle informatie die nodig is voor het draaien van het programma. Je kan het programma hier zelfs compileren. In de configuratie geef je die directory(tree) door, en de inhoud wordt gezipt in de configuratie en dus de JSON opgenomen. Op RUNTIME wordt die directory(tree) zip uitgepakt voordat het proces gedraaid wordt.
#### Q: Wat lever je uiteindelijk op?
#### A: In de git-template zitten diverse projecten. Je zult het meest in Description.{Domain} bezig zijn en Develop.{Domain} draaien. Maar het doel is een containerimage per omgeving te maken wat met jouw Dockerfile ook het programma Run.{Domain} en de ServiceDefinitions.*.aes bevat. 
#### Q: Welke controles vinden er plaats voordat een container image wordt gemaakt?
#### A: Dat zijn de eigen controles in de "CreateServiceDefinition" functies, fouten bij het koppelen van serviceversies aan "Wijzigingen", Resources wel overeenkomen met de omgeving, resource-sleutels werken, en voor de testomgevingen het proces met testdata goed verloopt. De testen draaien op het development image en niet op het RUNTIME image. Met het RUNTIME image kan in de praktijk getest worden in de test-omgeving(en). Het container image wordt nog gescand op kwetsbaarheden.
#### Q: Ik ontwikkel in C#. Moet ik allemaal aparte C# programma's voor mijn stappen aanmaken?
#### A: Nee, Er is een apart project waarin je losse C# functies kan schrijven. Deze functies kan je als aparte programma stap laten uitvoeren in het domein proces.
#### Q: Hoe pas je aangekochte software in.
#### A: Dat is afhankelijk van wat die software doet. Luistert de software naar poorten en is toegang nodig voor handmatig onderhoud dan moet het op een apart systeem komen te staan, en moet je het in je eigen software beschouwen als apart (onderdeel van een) domein. Biedt de externe software een Api, gebruik dan een Api-key ter beveiliging. Het externe systeem mag een Api van jou gebruiken maar mag nooit weten waar die logica geïmplementeerd is, uitwisseling via shared filesystemen is heel onveilig, gebruik dan een eigen stukje software om vanaf het lokale filesysteem via een storage systeem de gegevens uit te wisselen. Je kunt vragen of de leverancier een container image van zijn software oplevert en onderhoudt en wat het doet en wat jij moet doen om het veilig te maken. Dan heb je ook goed zicht op de poorten die het gebruikt. Probeer te voorkomen dat de externe software direct jouw beveiligde bronnen benadert als het geen mechanisme heeft om de sleutel daarvan te beschermen. In (heel)sommige gevallen kan je de externe software wel op jouw eigen systeem zetten, maar dan moet het passen in het patroon van Immutable, Stateless en Indirecte-Communicatie.
### Over beveiliging
#### Q: Wat kan een kwaadwillende met jouw project sources?
#### A: In principe niks, om iets te kunnen is toegang nodig tot de sleutelkluis van het domein.
#### Q: Wat kan een kwaadwillende met jouw container image?
#### A: Hij kan het draaien en hij kan proberen de configuratie te ontcijferen. Het eerste betekent dat hij jouw logica gaat draaien en zolang je beveiligd ben tegen dubbel draaien of dat dubbel draaien mogelijk is, is er geen probleem. Dat laatste is vervelender, iemand met tijd zou de verborgen sleutels in de configuratie kunnen achterhalen. Daarom moet je de sleutels periodiek veranderen.
#### Q: Is er toegang tot de sleutelkluis op RUNTIME?
#### A: Nee, alleen op ontwikkeltijd, de sleutels worden overgenomen in de versleutelde configuratie, je hoeft niet bang te zijn dat iemand via een (oud) RUNTIME image aan jouw actuele sleutels kan komen.
#### Q: Kan je het Run.{Domain} programma draaien met een andere configuratie?
#### A: Nee, die twee zijn aan elkaar gekoppeld, los van elkaar werken ze niet.
#### Q: Kan je tijdelijke bestanden versleutelen?
#### A: Ja, als je niet via streams kan werken kan je de bestanden via een tijdelijke sleutel versleutelen en ontcijferen. De sleutel kan telkens bij de start van het proces aangemaakt worden en via het environment of als parameter worden doorgegeven aan de stappen die hem nodig hebben.
#### Q: Heb je om te monitoren toegang nodig tot het RUNTIME systeem?
#### A: Nee, dat is niet nodig, dankzij het patroon is er goede generieke monitoring mogelijk, wat het proces precies doet is niet belangrijk wel of het periodiek gestart wordt en of de uitvoering wel op tijd goed is gegaan. Dat "dashboard" wordt gepubliceerd en een dashboard programma (met de juiste toegangssleutel) kan zich daarop abonneren. 
#### Q: Over wat voor systemen gaat het?
#### A: Het gaat over interne systemen die voor niemand zichtbaar hoeven te zijn. Dat zijn over het algemeen achtergrond processen die periodiek actief worden of starten op basis van een gebeurtenis. Heb je echter systemen die een GUI of een API implementeren dan zou ik voor de publieke toegang gebruik maken van een gateway, dus uitbesteden aan een goede poortwachter die alleen betrouwbaar verkeer doorstuurt naar jouw servers. Is het verzoek van een individuele klant dan zou ik het identificeren ook uitbesteden door gebruik te maken van het identificatie systeem van bijvoorbeeld een Cloud provider. Is het een API voor intern of extern gebruik maar niet gericht op een individuele klant, dan zou ik nog een API-key instellen. Voor alle systemen moet degene of het programma die een resource wil benaderen, expliciet een sleutel gebruiken. Uiteraard geen state bijhouden op de server zelf en voor Failover en Load Balancing zou je Kubernetes of iets soortgelijks kunnen gebruiken.
#### Q: Ik hoor je nergens over firewalls en netwerken, hoe zit dat?
#### A: Ik vind dat je moet programmeren alsof alles open is, dan moet het systeem ook veilig zijn. Maar extra beveiliging geeft meer zekerheid, dus waar iets niet publiekelijk toegankelijk hoeft te zijn, kan je het inderdaad beter verstoppen in een intern netwerk. Je moet echter niet alleen afhankelijk zijn van dat soort beveiliging. 
### Over mij
#### Q: Heb je hier wel ervaring mee?
#### A: Ik ben al lang softwareontwikkelaar en ik ben ook al lang bezig met deze zogenaamde "achtergrond" systemen, en het automatiseren van de ontwikkeling hiervan. Ik vind het jammer dat bij het opleveren van een draaiend systeem, er nog stukken met de hand worden gedaan, niet traceerbaar waardoor er ruimte ontstaat om bewust of onbewust fouten te maken. Ik vind dat het niet alleen een afspraak moet zijn, je moet het ook onmogelijk maken om buiten het source control systeem iets te wijzigen. Traceerbaarheid is de sleutel tot zowel de kwaliteit als de veiligheid. Daarnaast moet het opgeleverde systeem zo onafhankelijk mogelijk zijn, niet gevoelig voor iets waar het zelf geen invloed op kan uitoefenen en waardoor het ineens op mysterieuze wijze kan disfunctioneren. 
#### Q: Ben je een beveiligingsexpert?
#### A: Nee, ik kan goed gebruik maken van wat anderen mij bieden aan beveiliging opties en ik beperk bewust de toegang tot mijn systeem. Als de deur er niet is dan hoef ik hem ook niet te beveiligen. Sterker, het systeem wordt opgeleverd, maar ik hoef daar nooit meer te zijn. Aan de buiten kant zie ik of het functioneert, kan ik het niet zien of functioneert het echt niet, dan vervang ik het systeem door een andere instantie. Misschien dat iemand toch zo slim is om toegang te krijgen, wees nooit te zelfverzekerd en hou er rekening mee, daarom is alles van waarde op het systeem versleuteld, daarmee zijn de gegevens en het proces tijdelijk beveiligd. Iemand zou die versleuteling kunnen ontcijferen, binnen de tijd die daarvoor nodig is, moet jij de sleutels hebben aangepast en heeft de hacker waardeloze informatie. Je weet niet hoever iemand daarmee is, dus moeten jouw sleutels periodiek gewijzigd worden.  Expert? Noem het boerenverstand, en gebruik (verzin ze niet zelf) de juiste technieken en instanties en laat je daarop testen. En vooral alles automatiseren, bij handmatige handelingen kan iemand wat over het hoofd zien, maar een automatisme is uitgedacht, gecontroleerd en getest, niet gevoelig voor stress en zorgt voor traceerbare uitvoering. 
#### Q: Wat wil je bereiken?
#### A: Natuurlijk betere en veiligere systemen. De manier waarop vind ik belangrijk, die betere systemen zullen niet ontstaan omdat alle vinkjes gezet zijn na een handmatige controle. We zullen na iedere verandering en na verloop van tijd de systemen moeten testen en controleren op werking en kwetsbaarheden. Dat kan niet handmatig, dat is te gevoelig voor individueel falen, dat moet geautomatiseerd op basis van de kennis van velen. We mogen dat niet loslaten als het tegenzit, als ondanks de zorgvuldigheid, het systeem niet doet wat we willen of bedreigd wordt. Bedreigingen afwenden hoort een routine te zijn, ook als je er niks van merkt kan je bedreigd worden dus wend je die virtuele dreiging periodiek af, net zoals bij een echte dreiging. Bij een slecht functionerend systeem, vervang je het hele systeem door een corrigerend systeem indien nodig, waarna het correctie systeem weer vervangen wordt door een goed werkende versie. Het versie systeem is heel krachtig, een nieuwe versie kan een service uitschakelen door geen definitie op te leveren, de volgende versie gebruikt de definitie van de voorlaatste en schakelt de service weer in. Zo kan je noodzakelijke conversies inpassen. Je gaat met de versies altijd vooruit en nooit terug, zo blijft het traceerbaar wat er gebeurd is, zelfs de correctie of conversie is een gewone aanpassing op het systeem. De chronologie moet zichtbaar zijn in de sources en dat moet compleet zijn. 
#### Q: Waar zou je ons mee kunnen helpen?
#### A: Helpen met het ontwerp van een betrouwbaar en veilig systeem, en ondersteuning geven bij het gebruiken van (moderne) technieken en producten om dat te bereiken. Voor een goed werkend en veilig systeem moet alles kloppen tot aan de kleinste details toe. Om dat te bereiken is een goed ontwerp en de daarop gebaseerde formele definitie nodig waardoor je hulpmiddelen kan gebruiken die vroegtijdig fouten signaleren en assisteren bij het correct ontwikkelen en testen. Dat is niet nieuw, zo ontwikkelen wij onze programma's, maar nu gebruik je het om een volledig systeem op te tuigen waar zo'n programma maar een onderdeeltje van is. Ik heb een ontwerp en een formele definitie, je mag ze gebruiken als uitgangspunt.  