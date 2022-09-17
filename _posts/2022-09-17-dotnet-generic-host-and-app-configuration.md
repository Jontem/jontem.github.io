---
title: .NET Generic host configuration
date: 2022-09-17T00:00:00+00:00
layout: post
permalink: /dotnet-generic-host-and-app-configuration/
categories:
  - dotnet
tags:
  - dotnet
  - c#
---

A .NET generic host is an object that encapsulates an app's resources and lifetime functionality like dependency injection, logging and configuration.

One example when a generic host is useful is when you want to create a worker which fetches jobs from some kind of message queue.

The code below creates an `IHost` instance configured with some defaults and an implementation of a `IHostedService`

```cs
await Host.CreateDefaultBuilder(args)
    .ConfigureServices(services =>
    {
        services.AddHostedService<SampleHostedService>();
    })
    .Build()
    .RunAsync();
```

The `CreateDefaultBuilder` creates an `IHostBuilder` object which comes with some default configuration set.

- The content root is set to the result of `Directory.GetCurrentDirectory()`
- It loads host configuration from command-line and environment variables prefixed with `DOTNET_`
- App configuration is loaded from:
  - `appsettings.json`
  - `appsettings.{Environment}.json`
  - User secrets
  - Environment variables
  - Command-line arguments
- Some default logging providers are activated

So what is the difference between `HostConfiguration` and `AppConfiguration`

Host configuration is used for configuring the properties of the `IHostEnvironment` implementation. This interface has properties like:
- `EnvironmentName` - (Development/Production)
- `ContentRootPath` - The base path for the executable hosting the app and content files like configuration files. The base path is the default for the `JsonConfigurationProvider` which is used when build the `AppConfiguration`.

Host configuration is available inside `ConfigureAppConfiguration`
on the `HostBuilderContext.Configuration` object. When `AppConfiguration` is built the `HostBuilderContext.Configuration` is replaced with the app configuration

Host configuration can be configured with `ConfigureHostConfiguration`. Environment variables with the prefix `DOTNET_` will be loaded into host configuration by default. 

App configuration is configured by calling `ConfigureAppConfiguration` on `IHostBuilder` and is then available HostBuilderContext.Configuration and can be accessed with requiring `IConfiguration` interface with the help of dependency injection.

## Conclusion
The host configuration is used to configure the generic host but also as a way to control the outcome of `ConfigureAppConfiguration`. I guess the reason to split these up in two steps is to be able to configure more how the application configuration should look like depending on the hosting environment. One example could be if you want to load some other `JsonConfiguration` sources for development or production.



