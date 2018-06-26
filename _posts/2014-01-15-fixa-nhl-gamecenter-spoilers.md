---
id: 175
title: Fixa NHL GameCenter spoilers
date: 2014-01-15T17:58:32+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=175
permalink: /fixa-nhl-gamecenter-spoilers/
image: /wp-content/uploads/2014/01/download.jpeg
categories:
  - Onlinetjänster
  - Tips och trix
tags:
  - GameCenter
  - NHL
  - Viaplay
---
NHL GameCenter tycker jag är klockrent men det finns en liten sak som stör mig och det är att man kan se om matchen avgörs på ordinarie tid,(Final) förlängning(Final OT) eller straffar(Final SO).

[<img class="alignnone size-full wp-image-176" alt="Screen Shot 2014-01-15 at 17.12.14" src="//www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.12.14.png" width="491" height="116" srcset="https://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.12.14.png 491w, https://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.12.14-300x70.png 300w" sizes="(max-width: 491px) 100vw, 491px" />](http://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.12.14.png)

Detta syns även om man valt att dölja resultat men som tur är går det att åtgärda med hjälp av ett plugin som heter Stylish. Med hjälp av Stylish kan vi manipulera den css-fil som kommer med webbsidan. De webbläsare som stöds är Chrome och Firefox

  1. Hämta Stylish här: [Firefox](https://addons.mozilla.org/en-US/firefox/addon/stylish/?src=external-userstyleshome) eller [Chrome](https://chrome.google.com/webstore/detail/fjnbnpbmkenffdnngjfgmeleoegfcffe)
  2. När du har hämtat hem och installerat Stylish. Logga in på viaplay.se och gå sedan vidare till NHL GameCenter. Klicka sedan på Stylishikonen och välj Manage installed styles
  
    [<img class="alignnone size-full wp-image-178" alt="Screen Shot 2014-01-15 at 17.24.34" src="//www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.24.34.png" width="246" height="137" />](http://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.24.34.png)st
  3. Välj sedan Write new style
  4. **För Chrome
  
** Under code klistra in detta:</p> 
    <pre>#scoreStrip.scrsOff .offvalue {

display: none !important;

}</pre>
    
    Under Applies to välj URLs matching the Regexp och klistra sedan in denna url:
  
    http://viasat.neulion.com/viasat/gclplayer.jsp
    
    Längst till vänster fyller du sedan ett lämpligt namn för att spara och glöm inte bocka i enabled.
    
    **För Firefox**
  
    Välj ett valfritt namn. Klistra sedan in detta i den stora rutan:
    
    <pre>@-moz-document regexp("http://viasat.neulion.com/viasat/gclplayer.jsp") {
#scoreStrip.scrsOff .offvalue {
	display: none !important;
}

}</pre>

  5. Save
  6. Om du uppdaterar GameCenter-sidan så bör det nu se ut så här:
  
    [<img class="alignnone size-full wp-image-179" alt="Screen Shot 2014-01-15 at 17.36.25" src="//www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.36.25.png" width="491" height="108" srcset="https://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.36.25.png 491w, https://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.36.25-300x65.png 300w" sizes="(max-width: 491px) 100vw, 491px" />](http://www.mourtada.se/wp-content/uploads/2014/01/Screen-Shot-2014-01-15-at-17.36.25.png)

Tyvärr syns det inte när matcher börjar med denna fixen. Det går dock att få fram genom att sätta scores till &#8220;On&#8221;.

&nbsp;