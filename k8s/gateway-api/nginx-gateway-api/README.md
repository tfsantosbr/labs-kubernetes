# NGINX GATEWAY API

## Installation

Follow this [Installation Guide](https://docs.nginx.com/nginx-gateway-fabric/installation/installing-ngf/manifests/) to install NGINX gateway API before running the next commands.

## Configure NGINX Gateway API for Applications

```bash
kubectl apply -f k8s/gateway-api/nginx-gateway-api/namespace.yml
kubectl apply -f k8s/gateway-api/nginx-gateway-api
```

Check if the resources are up:

```bash
# Checking nginx load balance service
kubectl get svc nginx-gateway -n nginx-gateway

# Checking the application resources
kubectl get pod,deploy,svc,gateway,secrets -n nginx-gateway-api-ns
```

## Testing Applications

Testing applications with HTTPs:

```bash
curl -k https://app1.example.local # APP v1
curl -k https://app2.example.local # APP v2
```

Testing HTTP redirection to HTTPs:

```bash
curl http://app1.example.local # 302 Found
curl http://app2.example.local # 302 Found
```

## Canary Deploy Strategy

Run the following command in separate terminal to see results in real time:

```bash
while true; do curl -k https://app.canary.example.local; sleep 1; done
```

Modify the properties `weight` of file [app-canary-http-route.yml](./app-canary-http-route.yml) between 0 and 100, and see te traffic routing to service with greattest weight

```yml
# app-canary-http-route.yml
backendRefs:
  - name: app-v1-service
    weight: 50 # modify to change the weight for app-v1-service traffic
  - name: app-v2-service
    weight: 50 # modify to change the weight for app-v2-service traffic
```

## A/B Test Deploy Strategy

The [app-abtest-http-route.yml](./app-abtest-http-route.yml) has the rules for A/B test deploy strategy. The rule is configured to allow requests to app-v2-service when Header `group: beta-test-users` is present.

```yml
rules:
  - backendRefs:
      # if no specific header is present, default traffic send to app-v1-service
      - name: app-v1-service
        port: 80
  - matches:
      # send traffic to app-v2-service, when header 'group: beta-test-users' is present
      - headers:
          - name: group
            value: beta-test-users
    backendRefs:
      - name: app-v2-service
        port: 80
```

For test this rule, you can run these following commands:

```bash
curl -k https://app.example.local # APP v1 (default traffic)
curl -k -H "group: beta-test-users" https://app.example.local # APP v2 (header match rule traffic)
```

## Blue/Green Deploy Strategy

The [app-blue-green-http-route.yml](./app-blue-green-http-route.yml) has the rules for Blue/Green deploy strategy. The rule is based on host and when users access the `https://app.blue.example.local` URL, the traffic is routed to `app-v1-service` and when users access the `https://app.green.example.local` URL, the traffic is routed to `app-v2-service`.

```yml
hostnames:
  - app.blue.example.local
rules:
  backendRefs:
    - name: app-v1-service
      port: 80
---
hostnames:
  - app.green.example.local
rules:
  backendRefs:
    - name: app-v2-service
      port: 80
```

To test this rule, you can run these following commands:

```bash
curl -k https://app.blue.example.local # APP v1
curl -k https://app.green.example.local # APP v2
```

## Remove Application Resources

```bash
kubectl delete -f k8s/gateway-api/nginx-gateway-api
```

## Uninstall NGINX Gateway API

Follow this [Uninstallation Guide](https://docs.nginx.com/nginx-gateway-fabric/installation/installing-ngf/manifests/#uninstall-nginx-gateway-fabric) to uninstall NGINX gateway API
