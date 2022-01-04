---
title: What is OpenTelemetry?
description: This is the first post in a planned blog series about OpenTelemetry.
date: 2021-12-04T00:00:00+00:00
layout: post
permalink: /what-is-opentelemetry/
categories:
  - OpenTelemtry
tags:
  - OpenTelemetry
  - Observability
  - Monitoring
  - Distributed tracing
---

This is the first post in a planned blog series about OpenTelemetry.

## What is OpenTelemetry?
OpenTelemetry is an open source observability framework which is the result of merging the projects OpenTracing and OpenCensus. It's goal is to provide language agnostic tools for telemetry data such as tracing, metrics and logging in distributed architectures.

## Signals
The architecture of OpenTelemetry is designed around signals and can be thought of as categories of telemetry. Each signal provides a way for software to describe itself.

There are four signals defined in the specification:
* Tracing
* Metrics
* Logs
* Baggage

## Tracing
A trace tracks the progress of an request as it's flows through services in a distributed application. A trace contains a tree of spans. A span represents a unit of work for example in a service or a component.

## Metrics
A metric is measurement of work being done by a service or a component in a specific point in time. An example of a metric can be a counter of total requests for a http endpoint.

## Logs
A log entry is a text record that has a timestamp. Logs is an independant data source but can also be attached to spans.

## Baggage
Bagagge is a mechanism for propagating observability events as name/value pairs in a distributed transaction. This can be used to store data about a trace across process-boundaries.

## Context propagation
Signals are built on shared a mechanism called context propagation. The context provides a way to store state and accessing data over the lifetime of a distributed transaction. Propagators are used to serialize and deserialize context over different protocols. 

## Data collection
The OpenTelemtry Collector is a vendor-agnostic implementation of receiving, processing and exporting of telemetry data. This can for example be run on every node in kubernetes to collect or receive telemetry data for containers running on that node.