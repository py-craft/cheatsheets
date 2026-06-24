# Kubectl command cheatsheet

Get resource with additional information
```
kubectl get deployment redis -o wide
```

Create yaml definition with dry-run
```
kubectl create deployment --dry-run=client --image=nginx nginx-deployment --output=yaml > deployment.yaml
```

Change context to another namespace
```
kubectl config set-context $(kubectl config current-context) --namespace ingress-nginx
```

To limit resources per namespace, you should configure resource quota.
Create a file with filename `resource_quota.yaml`

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-resources
  namespace: default
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

```
kubectl apply -f resource_quota.yaml
```

In order to access to the service (db-service) in another (dev) namespace you need to use following dns

```
db-service.dev.svc.cluster.local
```

For scaling deployment replicas

```
kubectl scale deployment nginx --replicas=4
```