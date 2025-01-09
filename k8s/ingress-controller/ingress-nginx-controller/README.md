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
- route:
    - destination:
        host: app-service
        subset: v1
      weight: 50 # modify to change the weight for app v1 traffic
    - destination:
        host: app-service
        subset: v2
      weight: 50 # modify to change the weight for app v2 traffic
```

## A/B Test Deploy Strategy

The [app-abtest-virtual-service.yml](./app-abtest-virtual-service.yml) has the rules for A/B test deploy strategy. The rule is configured to allow requests to app-v2-service when Header `group: beta-test-users` is present.

```yml
http:
  # send traffic to app-v2-service, when header 'group: beta-test-users' is present
  - match:
      - headers:
          group:
            exact: beta-test-users
    route:
      - destination:
          host: app-service
          subset: v2
    # if no specific header is present, default traffic send to app-v1-service
  - route:
      - destination:
          host: app-service
          subset: v1
```

For test this rule, you can run these following commands:

```bash
curl -k https://app.example.local # APP v1 (default traffic)
curl -k -H "group: beta-test-users" https://app.example.local # APP v2 (header match rule traffic)
```

## Blue/Green Deploy Strategy

The [app-blue-green-virtual-service.yml](./app-blue-green-virtual-service.yml) has the rules for Blue/Green deploy strategy. The rule is based on host and when users access the `https://app.blue.example.local` URL, the traffic is routed to `app-v1-service` and when users access the `https://app.green.example.local` URL, the traffic is routed to `app-v2-service`.

```yml
spec:
  hosts:
    - app.blue.example.local
  http:
    - route:
        - destination:
            host: app-service
            subset: v1
---
spec:
  hosts:
    - app.green.example.local
  http:
    - route:
        - destination:
            host: app-service
            subset: v2
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
