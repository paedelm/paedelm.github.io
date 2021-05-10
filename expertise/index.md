---
layout: default
title: Integratie van systemen
---
# {{ page.title }}

## Expertise

  Ik heb een passie voor het ontwerpen van processen die moeten samenwerken en gegevens moeten uitwisselen. 
  De onderverdeling in domeinen en een goed ontwerp van de interactie tussen hen is heel belangrijk voor het dagelijks functioneren en voor het kunnen aanbrengen van toekomstige aanpassingen. 
  
  > Systemen ontwerpen die helemaal automatisch runnen

  Vooral oudere systemen zijn ontworpen met de gedachte dat er wel iemand handmatig kan ingrijpen als er iets mis gaat. Het is de kunst om vooraf te kunnen zien wat er mis zou kunnen gaan. En als je dat weet dan kan je het wel voorkomen of automatisch corrigeren. 
  
  > Hoe minder je van elkaar weet des te beter. 
  
  Alleen op de raakvlakken (interfaces) van systemen zijn goede afspraken nodig, voor de rest zou het niet moeten uitmaken hoe een systeem zijn functionaliteit implementeert. Dan geeft het ook geen problemen als dat veranderd wordt. 

  > Ervaring is belangrijk, weet wat er fout kan gaan!

  Jouw domein moet de gegevens natuurlijk betrouwbaar verwerken, dat heb je van te voren goed getest, en je hebt ook rekening gehouden dat alles van buiten het domein, dat waar je geen invloed hebt, goed gecontroleerd wordt en dat je bij voorziene fouten een geautomatiseerde oplossing hebt bedacht. Het is natuurlijk niet voor 100 procent te voorkomen, maar als er iets gebeurt wat je niet had voorzien kan dat leiden tot een exceptie waarbij er handmatig ingegrepen wordt. En dat zijn momenten waar de veiligheid ook in gedrang komt. Ervaring en expertise is hier belangrijk, als je weet wat fout kan gaan en je hebt het ook fout zien gaan, dan is dat een goede start om de fout te voorkomen of er eventueel geautomatiseerd mee om te gaan.

  > Als de veiligheid niet in orde is, dan is de betrouwbaarheid ook ver te zoeken.

  Binnen het domein is veilig verwerken een eis. Laat nergens geheimen slingeren, je hebt spijt als iemand de toegang tot jouw belangrijke database uit een script of een configuratie file kon halen. Eigenlijk moet alles van waarde versleuteld zijn en de sleutel moet niet onder het bloempotje liggen.

  > [Scripts kunnen gevaarlijk zijn](https://tweakers.net/nieuws/180646/criminelen-stalen-inloggegevens-van-ontwikkelaars-via-devtool-bash-uploader.html), gebruik altijd de oorspronkelijke versie.

  Even een script aanpassen, een regeltje erbij voor logging, even aanpassen voor een nieuw ontstane situatie, het is verleidelijk en o zo makkelijk. Wie heeft het in de gaten? Hoe lang duurt het voordat je in de gaten hebt dat het script niet is zoals jij bedoeld had? Je moet jezelf beschermen tegen deze verleiding maar een hacker is moeilijker tegen te houden. Verzin iets dat het script niet zichtbaar op systeem staat, om te runnen komt het uit een versleutelde bron en wordt het via memory aan de interpreter aangeboden, of verzin iets anders. 

  > Leve de DevOps teams, en leve de ervaring.

  Veel systemen zijn nog uit de tijd dat een developer de software schreef en een beheerder dat installeerde. Soms ging dat installeren automatisch, of via een handleiding, of een combinatie. En als er dan wat fout ging, was het de beheerder die in de log en op het systeem kon kijken. En de developer zat met de handen in het haar, hij keek in Git en deed zijn test en alles was goed, maar in het echt ging het fout. Soms was de fout het verschil tussen wat in Git stond en wat er werkelijk in productie stond.

  Dankzij DevOps kan nu alles in Git worden vastgelegd. De verschillen tussen Test en Productie omgevingen zijn daar nu te vinden en zelfs de infrastructuur kan daar in Code worden vastgelegd. Nu is het mogelijk om een runtime omgeving op te bouwen vanuit Git en dat helemaal te automatiseren. Als de gegevens dan ook ergens anders zijn opgeslagen, en dat kan, zou het runtime systeem afgesloten kunnen worden voor interactief gebruik.  

  > Op naar betrouwbare en veilige systemen

  Ik heb een methodiek ontwikkeld die als basis kan dienen voor betrouwbare en veilige systemen. Het is bedoeld voor migratie van oude systemen maar ook voor nieuwbouw. Zie mijn verhaal over [_ "Het touwtje uit de brievenbus"_]({% post_url 2021-05-03-Touwtje-uit-de-brievenbus %}). 
