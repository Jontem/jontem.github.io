---
title: How to setup OpenTelemetry instrumentation in ASP.NET core
description: Test
date: 2022-01-28T00:00:00+00:00
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

## New project
Start by creating a new webapi project.
```sh
dotnet new webapi --output opentelemtry-example
```

## Add required packages
Then we need to add OpenTelemetry packages.
```sh
dotnet add package --prerelease OpenTelemetry.Exporter.Console
dotnet add package --prerelease OpenTelemetry.Extensions.Hosting
dotnet add package --prerelease OpenTelemetry.Instrumentation.AspNetCore
```
The first package is the console exporter which is for outputing telemetry data on to your local developer console. There are many other exporters like jaeger or the opentelemtry collector.

The Extension.Hosting package includes the extension methods `AddOpenTelemetryTracing` and `AddOpenTelemetryMetrics` which is used on `IServiceCollection`to setup OpenTelemetry.

The Instrumentation.AspNetCore package collects telemetry data about incoming web requests. It provides the `AddAspNetCoreInstrumentation` extension method for `TraceProviderBuilder` and `MeterProviderBuilder`

## Add tracing
```cs
builder.Services
    .AddOpenTelemetryTracing((builder) => builder
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter()
    );
```
Above we're adding tracing and configure resource attribute `service.name` to MyServiceName. We also add the AspNetCore instrumation library and that tracing should be exported to the console.

## Add metrics
```cs
builder.Services
    .AddOpenTelemetryMetrics(builder => builder
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter(options =>
        {
            options.MetricReaderType = MetricReaderType.Periodic;
            options.PeriodicExportingMetricReaderOptions.ExportIntervalMilliseconds = 5000;
        }));
```
We're also adding metrics and configure the resource attribute `service.name` to MyServiceName. Here we also add the AspNetCore instrumation library and that metrics should be exported to the console. 


## Setup
* Packages needed
* Examples
* Activity source
* Metrics
* Logging
