---
id: 195
title: Clustered och non-clustered index
date: 2014-03-08T18:46:07+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=195
permalink: /clustered-och-non-clustered-index/
categories:
  - Databas
tags:
  - Clustered
  - Databas
  - index
  - non-clustered
  - SQL
---
För att få ner tiden på SQL-frågor mot stora tabeller så krävs index på rätt kolumner. Men vad är skillnaden mellan ett clustered och ett non-clustered index?

###### Clustered

  * Alla rader i tabellen sorteras fysiskt efter de kolumner som ingår i indexet.
  * Det kan bara finnas ett clustered index per tabell.
  * En tabell sorteras enbart fysiskt när den innehåller ett clustered index.
  * En tabell som innehåller ett clustered index kallas för &#8220;clustered table&#8221;.

###### Non-clustered

  * Ett non-clustered index har en struktur som skiljer sig från tabellens och innehåller kolumnerna som ingår i indexet och en pekare till den rad som är aktuell.
  * En tabell som inte innehåller ett clustered index kallas för &#8220;heap&#8221; och sorterar inte sina rader fysiskt.
  * En pekare i ett non-clustered index kallas för &#8220;row locator&#8221;. En row locator ser olika ut beroende på ifall tabellen innehåller ett clustered eller non-clustered index. Är det en heap table så pekar row locatorn på raden. Ifall det är en clustred table så pekar row locatorn på key:n i clustred index.