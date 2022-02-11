---
title: OpenTelemetry tracing with nodejs and express
description: In this post I'll go through a simple example on how to setup OpenTelemetry tracing in a nodejs express application.
date: 2022-02-11T00:00:00+00:00
layout: post
permalink: /opentelemetry-tracing-with-nodejs-and-express/
categories:
  - OpenTelemetry
tags:
  - nodejs
  - express
  - javascript
  - OpenTelemetry
  - OpenTelemetry collector
  - Observability
  - Monitoring
  - Distributed tracing
---

In this post I'll go through a simple example on how to setup OpenTelemetry tracing in a nodejs express application.

## Hello world express

```js
// index.js
const express = require("express");

const app = express();
const port = 3000;

app.get("/", (req, res) => {
  res.send("Hello World!");
});

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`);
});
```

## Needed packages

First we need to add the OpenTelemetry api and sdk package

```sh
yarn add @opentelemetry/api @opentelemetry/sdk-node
```

To instrument express, incoming and outgoing http requests we use the express and http instrumentation libraries.

```sh
yarn add @opentelemetry/instrumentation-http @opentelemetry/instrumentation-express
```

And lastly we add the otlp grpc tracing exporter.

```sh
yarn add @opentelemetry/exporter-trace-otlp-grpc
```

## Setup tracing

```js
// tracing.js
const OpenTelemetry = require("@opentelemetry/sdk-node");
const Resources = require("@opentelemetry/resources");
const SemanticConventions = require("@opentelemetry/semantic-conventions");
const InstrumentationHttp = require("@opentelemetry/instrumentation-http");
const InstrumentationExpress = require("@opentelemetry/instrumentation-express");
const ExporterTraceOtlpGrpc = require("@opentelemetry/exporter-trace-otlp-grpc");

const sdk = new OpenTelemetry.NodeSDK({
  resource: new Resources.Resource({
    [SemanticConventions.SemanticResourceAttributes.SERVICE_NAME]: "my-service",
  }),
  traceExporter: new ExporterTraceOtlpGrpc.OTLPTraceExporter({
    // url is optional and can be omitted - default is localhost:4317
    url: process.env["OTEL_EXPORTER_OTLP_ENDPOINT"] || undefined,
  }),
  instrumentations: [
    new InstrumentationHttp.HttpInstrumentation(),
    new InstrumentationExpress.ExpressInstrumentation(),
  ],
});

sdk.start();
```

# Run the express server
To configure and start the tracing sdk we can use nodes `-r/--require` flag to preload our tracing module.

```sh
node -r ./tracing.js index.js
```

Now every request will be traced and exported to the configured otlp receiver.
