---
id: 328
title: 'Azure AD DeleteAsync &#8220;Insufficient privileges to complete the operation.&#8221;'
date: 2015-03-04T12:22:24+00:00
author: jonte
layout: post
guid: http://www.mourtada.se/?p=328
permalink: /azure-ad-deleteasync-insufficient-privileges-to-complete-the-operation/
categories:
  - Azure
---
Jag sitter just nu med att skriva en website  med Angularjs och Web Api för att administrera användare i Azure AD. Under min katalog i Azureportalen har jag skapat upp en &#8220;web application&#8221; som har skriv- och läsrättigheter. Create, read och update funger fint men när jag skulle ta bort användare med metoden &#8220;DeleteAsync()&#8221; från klassen &#8220;ActiveDirectoryClient&#8221; då fick jag &#8220;Insufficient privileges to complete the operation.&#8221;

Som vanligt satte jag mig googlade och hittade denna tråd [User delete async result &#8220;Insufficient privileges to complete the operation.&#8221;](https://github.com/AzureADSamples/ConsoleApp-GraphAPI-DotNet/issues/5) på github. Med hjälp av denna lyckades jag lista ut hur man sätter extra behörigheter på en servicePrincipal via PowerShell.

  1. Installera dessa paket för att få tillgång till de PowerShellmoduler som behövs 
      * [Microsoft Online Services Sign-In Assistant for IT Professionals RTW](http://go.microsoft.com/fwlink/?LinkID=286152)
      * [Azure Active Directory Module for Windows PowerShell (64-bit version)](http://go.microsoft.com/fwlink/p/?linkid=236297)
  2. Öppna sedan upp &#8220;Windows Azure Active Directory-module&#8221; eller starta PowerShell som vanligt och importera modulen MsOnline &#8220;_Import-Module MSOnline_&#8220;
  3. Kör &#8220;_Connect-MsolService_&#8221; Du får nu upp en inloggningsruta där du fyller i dina uppgifter.
  4. Nästa steg är att först ta reda på sitt appid som är samma sak som clientid under applications i azure portalen eller via kommandot &#8220;_Get-MsolServicePrincipal_&#8220;. Nu kan du köra &#8220;_$app = Get-MsolServicePrincipal -AppPrincipalId <appid>_&#8220;
  5. Vi behöver även hämta rollen som vi vill lägga till applikationen i. Det gör vi med &#8220;_$role = Get-MsolRole -RoleName &#8220;Company Administrator&#8221;_&#8220;
  6. Nu lägger vi till applikationen i rollen med &#8220;_Add-MsolRoleMember -RoleObjectId $role.ObjectId -RoleMemberType ServicePrincipal -RoleMemberObjectId $app.ObjectId_&#8220;

Så nu ska applikationen fått rätt behörigheter för att kunna ta bort användare ur Azure AD. Jag hade dock problem med att jag fortfarande fick behörighetsfel men det räckte med att ta generera en ny secret till applikationen så fungerade det.