---
title: Setup gRPC ingress with nginx
date: 2022-03-04T00:00:00+00:00
layout: post
permalink: /setup-grpc-ingress-with-nginx/
categories:
  - kubernetes
tags:
  - kubernetes
  - ingress
  - nginx
  - grpc
---

This post shows how to setup gRPC via nginx ingress in kubernetes.

You need to add the annotations:
* `nginx.ingress.kubernetes.io/ssl-redirect: "true"`
* `nginx.ingress.kubernetes.io/backend-protocol: "GRPC"`

You are also required to have an ssl certicate on you ingress.

A complete ingress example:
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/backend-protocol: "GRPC"
  name: my-ingress
  namespace: my-namespace
spec:
  ingressClassName: nginx
  rules:
  - host: grpc-service.mydomain.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: grpc-server
            port:
              number: 80
  tls:
  - secretName: grpc-service.mydomain.com-cert
    hosts:
      - grpc-service.mydomain.com
```