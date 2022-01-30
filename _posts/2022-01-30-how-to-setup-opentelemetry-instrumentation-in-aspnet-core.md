---
title: How to setup OpenTelemetry instrumentation in ASP.NET core
description: In this post I'll show how to setup OpenTelemetry tracing, metrics and logging for AspNetCore on dotnet 6
date: 2022-01-30T00:00:00+00:00
layout: post
permalink: /how-to-setup-opentelemetry-instrumentation-in-aspnet-core/
categories:
  - OpenTelemetry
  - C#
tags:
  - asp.net core
  - dotnet
  - C#
  - OpenTelemetry
  - OpenTelemetry collector
  - Observability
  - Monitoring
  - Distributed tracing
---

In this post I'll show how to setup OpenTelemetry tracing, metrics and logging for AspNetCore on dotnet 6

## New project
Start by creating a new project. I'll be using the webapi template.
```sh
dotnet new webapi --output opentelemetry-example
```

## Add required packages
Then we need to add the OpenTelemetry packages.
```sh
dotnet add package --prerelease OpenTelemetry.Exporter.Console
dotnet add package --prerelease OpenTelemetry.Extensions.Hosting
dotnet add package --prerelease OpenTelemetry.Instrumentation.AspNetCore
```
The first package is the console exporter which is for outputing telemetry data on to your local developer console. For a real project you would probably use something like the jaeger or the otlp exporter.

The Extension.Hosting package includes the extension methods `AddOpenTelemetryTracing` and `AddOpenTelemetryMetrics` which is used on `IServiceCollection`to setup OpenTelemetry. It also adds the `LoggingBuilder` extension method `AddOpenTelemetry`.

The Instrumentation.AspNetCore package collects telemetry data about incoming web requests. It provides the `AddAspNetCoreInstrumentation` extension method for `TraceProviderBuilder` and `MeterProviderBuilder`

## Add tracing
```cs
builder.Services
    .AddOpenTelemetryTracing((builder) => builder
        // Configure the resource attribute `service.name` to MyServiceName
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        // Add tracing of the AspNetCore instrumentation library
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter()
    );
```

## Add metrics
```cs
builder.Services
    .AddOpenTelemetryMetrics(builder => builder
        // Configure the resource attribute `service.name` to MyServiceName
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        // Add metrics from the AspNetCore instrumentation library
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter(options =>
        {
            options.MetricReaderType = MetricReaderType.Periodic;
            options.PeriodicExportingMetricReaderOptions.ExportIntervalMilliseconds = 5000;
        }));
```

## Add logging
```cs
builder.Host
    .ConfigureLogging(logging => logging
        .ClearProviders()
        .AddOpenTelemetry(options =>
        {
            // Export the body of the message
            options.IncludeFormattedMessage = true;
            // Configure the resource attribute `service.name` to MyServiceName
            options.SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"));
            options.AddConsoleExporter(options =>
            {
                options.MetricReaderType = MetricReaderType.Periodic;
                options.PeriodicExportingMetricReaderOptions.ExportIntervalMilliseconds = 5000;
            });
        }));
```

## Complete example
```cs
using OpenTelemetry.Logs;
using OpenTelemetry.Metrics;
using OpenTelemetry.Resources;
using OpenTelemetry.Trace;

var builder = WebApplication.CreateBuilder(args);

// Add services to the container.

builder.Services.AddControllers();
// Learn more about configuring Swagger/OpenAPI at https://aka.ms/aspnetcore/swashbuckle
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen();

builder.Services
    .AddOpenTelemetryTracing((builder) => builder
        // Configure the resource attribute `service.name` to MyServiceName
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        // Add tracing of the AspNetCore instrumentation library
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter()
    );

builder.Services
    .AddOpenTelemetryMetrics(builder => builder
        // Configure the resource attribute `service.name` to MyServiceName
        .SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"))
        // Add metrics from the AspNetCore instrumentation library
        .AddAspNetCoreInstrumentation()
        .AddConsoleExporter(options =>
        {
            options.MetricReaderType = MetricReaderType.Periodic;
            options.PeriodicExportingMetricReaderOptions.ExportIntervalMilliseconds = 5000;
        }));

builder.Host
    .ConfigureLogging(logging => logging
        .ClearProviders()
        .AddOpenTelemetry(options =>
        {
            // Export the body of the message
            options.IncludeFormattedMessage = true;
            // Configure the resource attribute `service.name` to MyServiceName
            options.SetResourceBuilder(ResourceBuilder.CreateDefault().AddService("MyServiceName"));
            options.AddConsoleExporter(options =>
            {
                options.MetricReaderType = MetricReaderType.Periodic;
                options.PeriodicExportingMetricReaderOptions.ExportIntervalMilliseconds = 5000;
            });
        }));


var app = builder.Build();

// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI();
}

app.UseHttpsRedirection();

app.UseAuthorization();

app.MapControllers();

app.Run();
```