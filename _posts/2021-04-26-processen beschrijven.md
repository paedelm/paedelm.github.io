---
layout: post
title: "Beschrijven van processen om ze geautomatiseerd te draaien"
date: 2021-04-26
---

## Achtergrond

 In het kader van een betrouwbaar en veilig IT domein, is het idee om de processen in Git te beschrijven en met die beschrijving ze ergens anders geautomatiseerd te laten draaien. Vanwege dat automatisme zal de beschrijving getypeerd moeten worden. Eenduidigheid is nodig, je hebt geen mogelijkheid meer om te vragen "wat bedoel je". Er zijn heel wat eisen te stellen aan de beschrijving, denk aan de mogelijkheid van uitbreiden van het schema zonder oude beschrijvingen te herschrijven, ondersteuning in een IDE zoals intellisense, maken van eigen templates voor bekende proces patronen, eenvoudige syntax en nog meer. 
 
 Mijn keuze is niet op een export beschrijving gevallen maar op een programmeertaal die bekende export formaten kan uitspugen. Een programmeertaal omdat daar alles inzit voor hergebruik, conditioneel genereren, indelen in sourcefiles, gebruik van intellisense, pre-processing zoals compileren, controleren zoals testen en nog veel meer. Uiteindelijk ben ik op F# terecht gekomen het heeft de voordelen van een dotnet omgeving met zijn uitgebreide library, het heeft een eenvoudige syntax, het heeft string interpolatie voor templates, condities zijn expressies en geen statements, en het allerbelangrijkste is de [Discriminated Union](https://fsharpforfunandprofit.com/posts/discriminated-unions/). C# kwam in de buurt maar heeft nog niet alles wat F# kan. Ik heb me beperkt tot DotNet en daarbinnen gericht op sterk getypeerde talen met mogelijkheid tot reflectie. Het wil niet zeggen dat er niet iets anders is.
 
 
