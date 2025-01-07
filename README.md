# labs-kubernetes

```bash
kubectl apply -f k8s/namespace.yml
kubectl apply -f k8s/webapp

kubectl get all -n webapp-namespace
```

```bash
kubectl port-forward service/webapp-v1-service 8080:80 -n webapp-namespace
kubectl port-forward service/webapp-v2-service 8080:80 -n webapp-namespace
```
