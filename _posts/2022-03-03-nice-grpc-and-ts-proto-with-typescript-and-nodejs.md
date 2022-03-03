---
title: nice-grpc client and ts-proto with typescript and nodejs
date: 2022-03-03T00:00:00+00:00
layout: post
permalink: /nice-grpc-client-and-ts-proto-with-typescript-and-nodejs/
categories:
  - grpc
tags:
  - nodejs
  - typescript
  - nice-grpc
  - ts-proto
---

In this post I'll go through how to setup `nice-grpc` client together with `ts-proto` using `typescript` and `nodejs`.

## Installing dependencies

```sh
# gRPC library
$ yarn add nice-grpc

# For compiling protobuf files
$ yarn add protobufjs long
$ yarn add --save-dev grpc-tools ts-proto
```

## Compiling protos

Compiling a single proto file

```sh
$ grpc_tools_node_protoc \
--plugin=node_modules/.bin/protoc-gen-ts_proto \
--ts_proto_out=compiled-protos \
--ts_proto_opt=outputServices=generic-definitions,outputClientImpl=false,oneof=unions,snakeToCamel=false,esModuleInterop=true \
--proto_path=protos \
protos/my_service.proto
```

To compile multiple protos in a folder

```sh
$ find ./protos -name *.proto -exec grpc_tools_node_protoc \
--plugin=node_modules/.bin/protoc-gen-ts_proto \
--ts_proto_out=compiled-protos \
--ts_proto_opt=outputServices=generic-definitions,outputClientImpl=false,oneof=unions,snakeToCamel=false,esModuleInterop=true \
--proto_path=protos \
{} \\;
```

## Creating channel and a client

If you have the following proto file:

```proto
syntax = "proto3";

package nice_grpc.example;

service ExampleService {
  rpc ExampleUnaryMethod(ExampleRequest) returns (ExampleResponse);
}

message ExampleRequest {
  string messageRequest = 1;
}
message ExampleResponse {
  string messageResponse = 1;
}
```

Then calling the gRPC service looks like this:

```ts
import { createChannel, createClient, Client } from "nice-grpc";
import { ExampleServiceDefinition } from "./compiled_protos/my_service";

const channel = createChannel("localhost:8080");

const client: Client<typeof ExampleServiceDefinition> = createClient(
  ExampleServiceDefinition,
  channel
);
const result = await client.ExampleUnaryMethod({ messageRequest: "hello" });
```
