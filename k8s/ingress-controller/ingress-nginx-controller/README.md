# INGRESS NGINX OPEN SOURCE CONTROLLER

## Installation

Follow this [Installation Guide](https://kubernetes.github.io/ingress-nginx/deploy/)
to install Ingress NGINX Open Source Controller before running the next commands.

## Configure Ingress NGINX Open Source for Applications

```bash
kubectl apply -f k8s/ingress-controller/ingress-nginx-controller/namespace.yml
kubectl apply -f k8s/ingress-controller/ingress-nginx-controller
```

Check if the resources are up:

```bash
# Checking nginx load balance service
kubectl get all -n ingress-nginx

# Checking the application resources
kubectl get pod,deploy,svc,ingress,secrets -n ingress-nginx-controller-ns
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
kubectl delete -f k8s/ingress-controller/ingress-nginx-controller
```

## Uninstall NGINX Ingress Controller

Follow this [Uninstallation Guide](https://docs.nginx.com/ingress-nginx-controller/installation/installing-nic/installation-with-manifests/#uninstall-ingress-nginx-controller) to uninstall NGINX Ingress Controller
