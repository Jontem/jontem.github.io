---
title: nice-grpc client and ts-proto with typescript and nodejs
date: 2022-03-02T00:00:00+00:00
layout: post
permalink: /nice-grpc-and-ts-proto-with-typescript-and-nodejs/
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
To compile multiple protos in a folder
```sh
$ find ./packages/server/src/dhpro-service/protos -name *.proto -exec grpc_tools_node_protoc \ 
--experimental_allow_proto3_optional \ ?
--plugin=node_modules/.bin/protoc-gen-ts_proto \
--ts_proto_out=$npm_package_config_dhpro_proto_out_dir \
--ts_proto_opt=outputServices=generic-definitions,outputClientImpl=false,oneof=unions,snakeToCamel=false,esModuleInterop=true \
--proto_path=$npm_package_config_dhpro_proto_in_dir_shared
--proto_path=$npm_package_config_dhpro_proto_in_dir {} \\;
```
* Getting protos
* Generating with protoc and ts-proto (--proto-path multiple)
* Using find for multiple proto files
* Creating channel and a client
