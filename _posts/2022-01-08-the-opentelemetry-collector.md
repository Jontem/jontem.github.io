---
title: The OpenTelemetry collector
description: The OpenTelemetry collector provides a vendor-agnostic implementation on how to receive, process and export telemetry data.
date: 2022-01-08T00:00:00+00:00
layout: post
permalink: /the-opentelemetry-collector/
categories:
  - OpenTelemetry
tags:
  - OpenTelemetry
  - OpenTelemetry collector
  - Observability
  - Monitoring
  - Distributed tracing
---

The OpenTelemetry collector provides a vendor-agnostic implementation on how to receive, process and export telemetry data. The collector is a single binary that can be run as an agent or a gateway.

## Versions of the collector

There are two versions of the collector. The core version contains components that are the foundation of the collector such as configuration and the most common receivers, processors and exporters. The contrib version contains all of the components of core but also more vendor-specific and experimental receivers, processors and exporters.

## Collector components
### Receivers

A receiver is how data gets into the collector. Data can either be pushed or pulled. One receiver can support multiple data sources(traces, metrics and logs). An example of a receiver is an OpenTelemetry Protocol(OTLP) endpoint or a prometheus scraper.

### Exporters

Exporters sends data to one or more backends/destinations. Like the receiver an exporter can also be push or pull based and support multiple data sources. For example an exporter could provide a prometheus endpoint or send data to a jaeger backend.

### Processors

Processors handles data after it's received but before it's exported. An example of a processor is the batch processor which puts incoming data in batches. This makes for better compressing of the data and limits the number of outgoing connections.
