---
id: 75
title: Windows Server 2012 deduplication
date: 2013-05-29T17:22:36+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=75
permalink: /windows-server-2012-deduplication/
categories:
  - Windows Server
tags:
  - Deduplication
  - Windows Server 2012
---
En av många nya funktioner i Windows Server 2012 är deduplication. Dedup letar efter block på filsystemet som ser likadana ut och sparar bara en kopia. Övriga block tas bort och sparas som metadata. Om du installerar Data deduplication rollen på din maskin så får du med ett analysverktyg som hjälper dig att  uppskattar hur mycket disk som kan sparas. Verktyget heter ddpeval och kan köras mot lokala diskar eller shares.

Nedan ser ni en analys av en disk som innehåller mycket programfiler och många versioner av samma fil. Space savings percent är alltså 69! Det måste ändå anses vara väldigt bra.
  
[<img class="alignnone size-full wp-image-82" alt="ddpeval" src="//www.mourtada.se/wp-content/uploads/2013/05/ddpeval.png" width="874" height="341" srcset="https://www.mourtada.se/wp-content/uploads/2013/05/ddpeval.png 874w, https://www.mourtada.se/wp-content/uploads/2013/05/ddpeval-300x117.png 300w, https://www.mourtada.se/wp-content/uploads/2013/05/ddpeval-624x243.png 624w" sizes="(max-width: 874px) 100vw, 874px" />](http://www.mourtada.se/wp-content/uploads/2013/05/ddpeval.png)

Dedup jobbar inte i realtid utan kan gå som bakgrundsjobb när den tycker att det är lugnt eller på klocka när du bestämmer att den ska gå. Det finns möjlighet att exkludera både filtyper och även hela mappar. Man kan också välja att bara köra dedup på filer som inte ändrats under en viss tid. Default är 5 dagar.

Problem med söktider mot disk kan uppstå då data kan bli fragmenterat när dedup-jobbet möblerar om på filsystemet. Man har motverkat detta genom att cacha block som är &#8220;heta&#8221; och den kan lära sig mönster om vilka block som brukar accessas efter varandra och blocken kan på så sätt placeras nära varandra. Detta är ett extra jobb som måste scheduleras eller startas manuellt.**
  
** 

Microsoft säger att volymer med mycket aktivt data som alltid är öppet och konstant ändras inte är supportat. Exempelvis virtuella maskiner eller SQL volymer(förutom backup volymer som man istället rekommenderar).

Funktionen kräver alltså Windows Server 2012(Det går att pilla ihop det med Windows 8) och har du en disk som har dedup påslaget så går det att migrera den till annan Windows server 2012 med rollen installerad. Något att notera är att dedup inte fungerar på microsofts nya filsystem Resilient File System (ReFS) utan bara på NTFS.

Det är även viktigt att tänka på att man inte kan konvertera tillbaks disken om man överallokerar diskutrymmet. Samtidigt som man inte kan krympa volymer bara för att man sparat utrymme.

Jag har tyvärr inte haft möjligheten ännu att testa funktionen då våran backupmiljö som är baserad på Netbackup ännu inte har stöd för dedup-funktionen(Dom har inte ens fullt stöd för Windows Server 2012 ännu..). Men kommer antagligen att slå på funktionen till hösten på filservrar som har 2012 installerat.