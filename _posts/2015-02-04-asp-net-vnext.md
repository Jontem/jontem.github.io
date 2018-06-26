---
id: 284
title: ASP.NET vNext
date: 2015-02-04T22:45:41+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=284
permalink: /asp-net-vnext/
categories:
  - ASP.NET
  - 'C#'
tags:
  - ASP.NET 5
  - ASP.NET MVC
  - ASP.NET vNext
  - ASP.NET Web API
  - Owin
---
ASP.NET vNext är Microsoft&#8217;s nästa version av deras .NET-stack för webbapplikationer. Man bygger vidare på Owin-&#8220;tänket&#8221; och gör alla delar mer löst kopplade till varandra. System.web är nu borta som varit beroende av IIS och man kan nu välja om man vill köra self-hosting eller vill fortsätta köra med IIS.

<span style="text-decoration: underline;">Det</span> finns nu 3 olika runtime:

  * Full .NET CLR
  
    Standard runtime som kommer vara bakåtkompatibel
  * Core CLR (cloud-optimized runtime)
  
    En slimmad och helt modulär runtime. Där all alla delar som ska används läggs till som Nuget-paket. Du får en applikation som bara är beroende av de delar som du behöver. Dessutom så kan en applikation i Core CLR köra bredvid andra applikationer med olika versioner av runtime. Från att .NET har krävt runt 200MB är Core CLR endast 11MB stor.
  * Cross-Platform CLR
  
    Nu finns även stöd för att köra MVC, Web pages och Web Api via mono på Mac och Linux. Man jobbar ihop med mono communityn och kommer att släppa ett eget cross-plattform runtime CLR. Men tillsvidare kan man köra med Mono.

Web pages, MVC och Web Api kommer nu i ett enda framework &#8220;MVC 6&#8221;. All duplicerad kod exempelvis System.Web.MVC och System.Net.Http är nu ett minne blott.

Assembly-referencer är nu historia. Istället refererar du Nuget-paket som hanteras via en json-fil(project.json). Även Web.config är borta och istället kan man välja vart ifrån specifika inställningar för dina olika miljöer ska laddas t ex från json, XML eller miljövariablar.

ASP.NET vNext utvecklas open source och finns på [GitHub](https://github.com/aspnet). Vill du läsa mer om vNext gå in på [ASP.NET/vnext](http://www.asp.net/vnext/) där finns massa information och du kan även dra ner senaste CTP:n av Visual Studio 2015. RTW(Release to web) är planerat till Q2 2015.

Videon nedan visar de allra största nyheterna grundligt och jag tycker absolut att du ska kolla igenom den. Jag ser fram emot att jobba med vNext eller ASP.NET 5 som det igentligen kallas!