# GATEWAY API WITH INGRESS CONTROLLER

## Install

```bash
kubectl apply -f k8s/orders-gateway-nginx/infrastructure
kubectl apply -f k8s/orders-gateway-nginx/development
```

## Uninstall

```bash
kubectl delete ns orders-ns
kubectl delete ns certificate
```
