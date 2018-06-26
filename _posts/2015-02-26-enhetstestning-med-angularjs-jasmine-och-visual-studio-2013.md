---
id: 297
title: Enhetstestning med AngularJs, Jasmine och Visual studio 2013
date: 2015-02-26T15:15:34+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=297
permalink: /enhetstestning-med-angularjs-jasmine-och-visual-studio-2013/
categories:
  - AngularJS
  - JavaScript
  - TDD
tags:
  - AngularJs
  - Chutzpah
  - Jasmine
  - javascript
  - Visual studio 2013
---
AngularJs är skrivet från början med enhetstestning i åtanke och har inbyggt stöd för dependency injection. Större delen av den officiella dokumentation innehåller exempel på hur man enhetstestar de olika komponenterna.

Det populäraste ramverket för att skriva enhetstest med AngularJs är Jasmine och alla officiell dokumentation är skriven i detta ramverk. Jasmine hjälper dig att hålla testen strukturerade och för att göra &#8220;assertions&#8221;.

Tyvärr säger inte den officiella dokumentationen något om hur man kan integrera testen i visual studios &#8220;Test explorer&#8221;. Efter att ha letat runt lite på internet så hittade jag en ganska enkel lösning

Börja med att skapa upp ett tomt Web-projekt och lägg till angular och jasmine.js via nuget. Skapa sedan upp ett tomt projekt av typen class library.<figure id="attachment_299" style="width: 299px" class="wp-caption alignnone">

[<img class="wp-image-299 size-full" src="https://www.mourtada.se/wp-content/uploads/2015/02/solution_explorer.png" alt="solution explorer" width="299" height="424" srcset="https://www.mourtada.se/wp-content/uploads/2015/02/solution_explorer.png 299w, https://www.mourtada.se/wp-content/uploads/2015/02/solution_explorer-212x300.png 212w" sizes="(max-width: 299px) 100vw, 299px" />](https://www.mourtada.se/wp-content/uploads/2015/02/solution_explorer.png)<figcaption class="wp-caption-text">Solution explorer</figcaption></figure> 

Installera sedan dessa extensions

  * [Chutzpah Test Adapter for Visual Studio](http://visualstudiogallery.msdn.microsoft.com/f8741f04-bae4-4900-81c7-7c9bfb9ed1fe)
  * [Chutzpah Test Runner Context Menu for Visual Studio](http://visualstudiogallery.msdn.microsoft.com/71a4e9bd-f660-448f-bd92-f5a65d39b7f0)

Vi lägger först till en ny app &#8220;TestApp&#8221; med en controller &#8220;testController&#8221;<figure id="attachment_315" style="width: 376px" class="wp-caption alignnone">

[<img class="wp-image-315 size-full" src="https://www.mourtada.se/wp-content/uploads/2015/02/testController.png" alt="testController" width="376" height="89" srcset="https://www.mourtada.se/wp-content/uploads/2015/02/testController.png 376w, https://www.mourtada.se/wp-content/uploads/2015/02/testController-300x71.png 300w" sizes="(max-width: 376px) 100vw, 376px" />](https://www.mourtada.se/wp-content/uploads/2015/02/testController.png)<figcaption class="wp-caption-text">app.js</figcaption></figure> 

Chutzpah kan använda sig av &#8220;\_references.js&#8221; eller inställningsfilen Chutzpah.json för att hitta dependencices. Det enklaste är att skapa &#8220;\_references.js&#8221; i rooten på testprojektet. Dra in angular.js, angular-mocks.js, jasmine och dina egna javascriptfiler som ska användas. Det går att referera javascript i andra projekt genom att ange path=&#8221;../AngularWebApplication/app/app.js&#8221;. Tyvärr fungerar inte detta för hints/intellisense i visual studio. Vill du ha hints måste javascripten finnas i samma projekt.<figure id="attachment_310" style="width: 461px" class="wp-caption alignnone">

[<img class="wp-image-310 size-full" src="https://www.mourtada.se/wp-content/uploads/2015/02/references.png" alt="References.js" width="461" height="67" srcset="https://www.mourtada.se/wp-content/uploads/2015/02/references.png 461w, https://www.mourtada.se/wp-content/uploads/2015/02/references-300x44.png 300w" sizes="(max-width: 461px) 100vw, 461px" />](https://www.mourtada.se/wp-content/uploads/2015/02/references.png)<figcaption class="wp-caption-text">References.js</figcaption></figure> 

Lägg till en ny js-fil för ditt första test &#8220;testControllerTest.js&#8221; i testprojektet. Referera till &#8220;_references.js&#8221; längst upp. Chutzpah kommer att scanna dina projekt efter .js filer och testa att kompilera dom för att se ifall det är filer som innehåller test. Detta kan vara en väldigt långsam process om man har ett stort javascriptbibliotek. I sådana fall kan man ställa in i explicit i Chutzpah.json vilka filer som är testfiler<figure id="attachment_311" style="width: 486px" class="wp-caption alignnone">

[<img class="wp-image-311 size-full" src="https://www.mourtada.se/wp-content/uploads/2015/02/testControllerTest.png" alt="testControllerTest" width="486" height="298" srcset="https://www.mourtada.se/wp-content/uploads/2015/02/testControllerTest.png 486w, https://www.mourtada.se/wp-content/uploads/2015/02/testControllerTest-300x184.png 300w" sizes="(max-width: 486px) 100vw, 486px" />](https://www.mourtada.se/wp-content/uploads/2015/02/testControllerTest.png)<figcaption class="wp-caption-text">testControllerTest.js</figcaption></figure> 

Om allt har gått rätt till så ska du nu ha 2 stycken test i test explorer<figure id="attachment_304" style="width: 770px" class="wp-caption alignnone">

[<img class="wp-image-304 size-full" src="https://www.mourtada.se/wp-content/uploads/2015/02/test_explorer.png" alt="test_explorer" width="770" height="60" srcset="https://www.mourtada.se/wp-content/uploads/2015/02/test_explorer.png 770w, https://www.mourtada.se/wp-content/uploads/2015/02/test_explorer-300x23.png 300w" sizes="(max-width: 770px) 100vw, 770px" />](https://www.mourtada.se/wp-content/uploads/2015/02/test_explorer.png)<figcaption class="wp-caption-text">Test Explorer</figcaption></figure> 

&nbsp;

**Länkar**:

  * [Angular unit test documentation](https://docs.angularjs.org/guide/unit-testing)
  * [Chutzpah på github](https://github.com/mmanela/chutzpah)
  * [Chutzpah blogg](http://matthewmanela.com/)