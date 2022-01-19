---
title: How to setup OpenTelemetry instrumentation in ASP.NET core
description: 
date: 2022-12-04T00:00:00+00:00
layout: post
permalink: /how-to-setup-opentelemetry-instrumentation-in-asp.net-core/
categories:
  - OpenTelemetry
tags:
  - asp.net core
  - dotnet
  - OpenTelemetry
  - OpenTelemetry collector
  - Observability
  - Monitoring
  - Distributed tracing
---

In this post I'll show how to setup OpenTelemetry tracing, metrics and logging for AspNetCore on dotnet 6

# Setup
```sh
dotnet new webapi --output opentelemtry-example
```

# Add required packages
Then we need to add OpenTelemetry packages.
```sh
dotnet add package OpenTelemetry.Exporter.Console
dotnet add package --prerelease OpenTelemetry.Extensions.Hosting
dotnet add package --prerelease OpenTelemetry.Instrumentation.AspNetCore
```
The first package is the console exporter which is useful for testing on your local developer machine. It outputs the telemetry data to the console.

The Extension.Hosting package includes the exetension methods `AddOpenTelemetryTracing` and `AddOpenTelemetryMetrics` which is used on `IServiceCollection`to setup OpenTelemetry.

The Instrumentation.AspNetCore package which collects telemetry data about incoming web requests.

# Wiring it up
```cs
services.AddOpenTelemetryTracing((builder) => builder
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter()
    );
```



## Setup
* Packages needed
* Examples
* Activity source
* Metrics
* Logging
