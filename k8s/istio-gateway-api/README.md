# NGINX GATEWAY API

## Installation

Follow this [Installation Guide](#) to install NGINX gateway API before running the next commands.

## Configure NGINX Gateway API for Applications

```bash
kubectl apply -f k8s/istio-gateway-api/namespace.yml
kubectl apply -f k8s/istio-gateway-api
```

Check if the resources are up:

```bash
kubectl get pod,svc,httproute,secrets,gateway -n istio-gateway-api-ns
```

## Testing Applications

Testing applications with HTTPs:

```bash
curl -k https://app1.example.local # APP v1
curl -k https://app2.example.local # APP v2
```

Testing HTTP redirection to HTTPs:

```bash
curl http://app1.example.local -v # 301 Moved Permanently
curl http://app2.example.local -v # 301 Moved Permanently
```

## Remove Application Resources

```bash

```

## Uninstall NGINX Gateway API

Follow this [Uninstallation Guide](#) to uninstall NGINX gateway API
