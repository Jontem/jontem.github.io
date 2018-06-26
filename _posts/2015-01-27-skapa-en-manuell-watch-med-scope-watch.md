---
id: 271
title: Skapa en manuell watch med $scope.$watch
date: 2015-01-27T22:27:41+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=271
permalink: /skapa-en-manuell-watch-med-scope-watch/
categories:
  - AngularJS
  - JavaScript
---
Signaturen för $watch:

<pre lang="javascript">$watch(watchExpression, listener, [objectEquality]);</pre>

  * WatchExpression är antingen ett angular expression eller en anonym function() som returnerar ett värde,
  * Listener är en anonym function som exekveras när watchern triggas.
  * ObjectEquality är en boolean som sätter hur värdena ska jämföras. False som är default innebär by reference. Med True är det by value.

Ett av grundkoncepten i Angular är **[Digest cycle](https://docs.angularjs.org/api/ng/type/$rootScope.Scope#$digest)**. Man kan säga att det är en loop som går igenom alla $scope:s watcher:s och kollar ifall det finns några förändringar på de variablar som övervakas. Exempel på vad som övervakas är när man anger något av dessa 3 sätt:

  * Har ett expression direkt i htmltemplate: ”{{ name }}”
  * Via Ng-model=”name” på ett input element
  * Eller manuellt via $scope.watch

De första alternativen registreras automatiskt som watchers av angular. Om någon watcher’s triggas på grund av att något är förändrat så sätts watcherns listener igång .  Observera att så fort en watcher triggats så initieras en ny digest cycle. Detta sker ända tills inga flera förändringar hittas.

Båda watch:erna nedan utför samma sak.

<pre lang="javascript">var app = angular.module('myApp', []);

app.controller('TestController', ['$scope', function($scope) {
    $scope.name = "Hej";

    $scope.$watch('name', function(newValue, oldValue) {
        alert(1);
    });

    $scope.$watch(function() {
        return $scope.name;
    }, function(newValue, oldValue) {
        alert(2);
    });
}]);</pre>

Vad är då syftet med att registrera en manuell watch? I vissa fall vill man att en viss procedur ska köras när ett värde förändras. Då är detta ett enkelt sätt att göra detta.

Om du vill läsa mer om hur scopes och digest, eval och apply med mera fungerar i angular. Kolla in detta välskrivna blogginlägg som beskriver hur man bygger ett &#8220;eget&#8221; angular:

<http://teropa.info/blog/2013/11/03/make-your-own-angular-part-1-scopes-and-digest.html>