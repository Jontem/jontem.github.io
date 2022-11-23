---
title: Kubernetes merge multiple ingresses
date: 2022-11-23T00:00:00+00:00
layout: post
permalink: /kubernetes-merge-multiple-ingresses/
categories:
  - kubernetes
tags:
  - nginx
  - k8s
  - kubernetes
---

The other day I wanted to point a subpath of an ingress to a service in another namespace. But an ingress can't access a service in another namespace. But I found that it can be achieved by declaring multiple ingresses and these can reside in different namespaces as long as they share same host. This is only available with nginx as the ingress controller.

If multiple ingresses share the same host then these are some of the rules that apply (https://kubernetes.github.io/ingress-nginx/how-it-works/#building-the-nginx-model)
- If more than one Ingress contains a TLS section for the same host, the oldest rule wins.
- If multiple Ingresses define different paths for the same host, the ingress controller will merge the definitions. 
- If the same path for the same host is defined in more than one Ingress, the oldest rule wins.

So if we define two ingresses like this:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-site-example-com-main
  namespace: a
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - site.example.com
    secretName: site-example-com-secret
  rules:
  - host: site.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: site-example-com-svc
            port:
              number: 80
```

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress-subpath-site-example-com
  namespace: b
spec:
  ingressClassName: nginx
  rules:
  - host: site.example.com
    http:
      paths:
      - path: /subpath
        pathType: Prefix
        backend:
          service:
            name: subpath-site-example-com-svc
            port:
              number: 80
```

These two ingresses will merge and the subpath ingress can be placed in another namespace and point to services in that namespace.

The [nginxinc/kubernetes-ingress](https://github.com/nginxinc/kubernetes-ingress) develop by NGINX supports something similar which is called [mergable ingress types](https://github.com/nginxinc/kubernetes-ingress/tree/v2.4.1/examples/ingress-resources/mergeable-ingress-types) which the community develop version [kubernetes/ingress-nginx](https://github.com/kubernetes/ingress-nginx/issues/3031) doesn't support.
