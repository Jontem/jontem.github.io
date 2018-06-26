---
id: 105
title: Raspberry PI
date: 2013-06-03T23:33:45+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=105
permalink: /raspberry-pi/
categories:
  - Linux
tags:
  - Arch linux
  - Linux
  - Rasbian
  - Raspberry PI
  - XBMC
---
Var i Stockholm i helgen och fick för mig att köpa en raspberry pi model B. Det är en &#8220;mini-pc&#8221; som är i storleken av ett kreditkort utan behov av kylning(Förutsatt att den inte är klockad). Den drivs via micro-usb och drar inte mer än 3.5W. Det är absolut inget monster med sin 700MHz arm-processor och 512MB ram. Till priset av ca 300kr är det en rolig liten pryl. Slutpriset blev dock lite mer(700kr) då jag behövde köpa ett SDHC-kort, micro-usbladdare och så ville jag ha ett litet fint chassi.

[<img class="alignnone size-full wp-image-106" alt="Raspberry PI" src="//www.mourtada.se/wp-content/uploads/2013/06/351321-raspberry-pi.jpg" width="600" height="500" srcset="https://www.mourtada.se/wp-content/uploads/2013/06/351321-raspberry-pi.jpg 600w, https://www.mourtada.se/wp-content/uploads/2013/06/351321-raspberry-pi-300x250.jpg 300w" sizes="(max-width: 600px) 100vw, 600px" />](http://www.mourtada.se/wp-content/uploads/2013/06/351321-raspberry-pi.jpg)

Började med att installera Rasbian som är det &#8220;rekommenderade&#8221; att börja med. Det bygger på Debian men är optimerat för raspberryns hårdvara. Rasbian kommer med LXDE som är ett grafiskt gränssnitt som fokuserar på att vara resurssnålt. Jag testade och kom snabbt fram till att det var inget jag skulle komma att använda. Responsen i gränssnittet var inte riktigt vad jag var van vid men det är samtidigt inte syftet att det ska vara det. Installerade istället Arch linux som inte kommer med något GUI och planerar att använda den som webbserver, utvecklingsserver och för VPN.

Det finns dock några intressant projekt om man vill använda den som media center. Det finns flera XBMC-portar som fungerar bra och till och med kan spela upp full-hd och DTS. Raspberry även har stöd för CEC som gör att man kan använda sin TV:s fjärrkontroll för att styra andra enheter via HDMI. Detta stöd finns i XBMC och fungerar i de portar som finns för raspberryn. Det kräver så klart att TV:n har stöd för detta.

En sak till som man bör hålla koll på är jobbet som pågår för att porta Android ICS. Det finns några betavarianter men inget som fungerar fullt ut. Som jag förstår det väntar man på att Broadcom levererar drivrutiner för att få fullt hårdvaruaccelereringsstöd. De versioner som finns nu ska vara oanvändbara.

Något som jag blev imponerad var hur enkelt det var att komma igång med de färdiga officiella image:ar som finns. I Windows är det bara att ladda ner imagefilen och skriva SDHC-kortet med &#8220;win32diskimager&#8221; som tar ca 5-10 minuter. Sedan var det bara att starta upp raspberryn och man är igång på under 5 minuter.

Om du inte ser något användningsområde för denna lilla pryl testa att google på &#8220;raspberry pi use cases&#8221; så kommer du att få se vad som går att göra med lite fantasi. Community:n runt raspberry:n är enorm och det känns som jag skulle kunna skriva flera a4 om den.

<http://www.raspberrypi.org/>