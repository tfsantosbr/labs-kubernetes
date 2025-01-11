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

## Canary Deploy Strategy

Run the following command in separate terminal to see results in real time:

```bash
while true; do curl -k https://app.canary.example.local; sleep 1; done
```

Modify the properties `weight` of file [app-canary-virtual-service.yml](./app-canary-virtual-service.yml) between 0 and 100, and see te traffic routing to service with greattest weight

```yml

```

## A/B Test Deploy Strategy

The [app-abtest-virtual-service.yml](./app-abtest-virtual-service.yml) has the rules for A/B test deploy strategy. The rule is configured to allow requests to app-v2-service when Header `group: beta-test-users` is present.

```yml

```

For test this rule, you can run these following commands:

```bash

```

## Blue/Green Deploy Strategy

The [app-blue-green-virtual-service.yml](./app-blue-green-virtual-service.yml) has the rules for Blue/Green deploy strategy. The rule is based on host and when users access the `https://app.blue.example.local` URL, the traffic is routed to `app-v1-service` and when users access the `https://app.green.example.local` URL, the traffic is routed to `app-v2-service`.

```yml
rules:
  - host: app.blue.example.local
    http:
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: app-v1-service
              port:
                number: 80
---
rules:
  - host: app.green.example.local
    http:
      paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: app-v2-service
              port:
                number: 80
```

To test this rule, you can run these following commands:

```bash
curl -k https://app.blue.example.local # APP v1
curl -k https://app.green.example.local # APP v2
```

## Remove Application Resources

```bash
kubectl delete -f k8s/ingress-controller/ingress-nginx-controller
```

## Uninstall NGINX Ingress Controller

Follow this [Uninstallation Guide](https://docs.nginx.com/ingress-nginx-controller/installation/installing-nic/installation-with-manifests/#uninstall-ingress-nginx-controller) to uninstall NGINX Ingress Controller
