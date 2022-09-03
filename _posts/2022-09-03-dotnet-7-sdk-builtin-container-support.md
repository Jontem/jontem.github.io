---
title: .NET 7 SDK builtin container support 
date: 2022-09-03T00:00:00+00:00
layout: post
permalink: /dotnet-7-sdk/builtin-container-support/
categories:
  - dotnet
tags:
  - .NET 7
  - dotnet
  - containers
---

In .NET 7 there will be built in support in the SDK for building containers. By running `dotnet publish --os linux --arch x64 -p:PublishProfile=DefaultContainer` msbuild will build you a container without the need for docker or any other container tool.

Read more about it here: [Announcing built-in container support for the .NET SDK](https://devblogs.microsoft.com/dotnet/announcing-builtin-container-support-for-the-dotnet-sdk/)

Here are some things worth noting:

- Right now this is an initial preview.
- You can't publish to a remote registry just yet. But this will be available with the full release of .NET7.
- There is no way to do `RUN` commands. You'll have to create base image and set the build property `ContainerBaseImage`.
- The way to configure things in the `Dockerfile` like entrypoint or environment variables is through msbuild project properties
- Right now linux x64 is the only supported container os. Windows containers will be supported with the full release of .NET 7
- Before the release of .NET 7 you'll need to install the nuget package to get this feature `dotnet add package Microsoft.NET.Build.Containers`

