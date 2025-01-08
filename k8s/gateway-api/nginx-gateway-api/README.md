# NGINX GATEWAY API

## Installation

Follow this [Installation Guide](https://docs.nginx.com/nginx-gateway-fabric/installation/installing-ngf/manifests/) to install NGINX gateway API before running the next commands.

## Configure NGINX Gateway API for Applications

```bash
kubectl apply -f k8s/nginx-gateway-api/namespace.yml
kubectl apply -f k8s/nginx-gateway-api
```

Check if the resources are up:

```bash
# Checking nginx load balance service
kubectl get svc nginx-gateway -n nginx-gateway

# Checking the application resources
kubectl get all,gateway,secrets -n nginx-gateway-api-ns
```

## Testing Applications

Testing applications with HTTPs:

```bash
curl -k https://app1.example.local # APP v1
curl -k https://app2.example.local # APP v2
```

Testing HTTP redirection to HTTPs:

```bash
curl http://app1.example.local # 301 Moved Permanently
curl http://app2.example.local # 301 Moved Permanently
```

## Remove Application Resources

```bash
kubectl delete ns nginx-gateway-api-ns
```

## Uninstall NGINX Gateway API

Follow this [Uninstallation Guide](https://docs.nginx.com/nginx-gateway-fabric/installation/installing-ngf/manifests/#uninstall-nginx-gateway-fabric) to uninstall NGINX gateway API
