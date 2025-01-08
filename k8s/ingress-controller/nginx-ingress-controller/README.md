# NGINX INGRESS CONTROLLER

## Installation

Follow this [Installation Guide](https://docs.nginx.com/nginx-ingress-controller/installation/installing-nic/installation-with-manifests/)
to install NGINX Ingress Controller before running the next commands.

## Configure NGINX Ingress for Applications

```bash
kubectl apply -f k8s/ingress-controller/nginx-ingress-controller/namespace.yml
kubectl apply -f k8s/ingress-controller/nginx-ingress-controller
```

Check if the resources are up:

```bash
# Checking nginx load balance service
kubectl get svc nginx-ingress -n nginx-ingress

# Checking the application resources
kubectl get pod,deploy,svc,ingress,secrets -n nginx-ingress-controller-ns
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
kubectl delete -f k8s/ingress-controller/nginx-ingress-controller
```

## Uninstall NGINX Ingress Controller

Follow this [Uninstallation Guide](https://docs.nginx.com/nginx-ingress-controller/installation/installing-nic/installation-with-manifests/#uninstall-nginx-ingress-controller) to uninstall NGINX Ingress Controller
