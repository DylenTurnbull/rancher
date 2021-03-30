# NGINX Plus Ingress Demo

Follow the directions below to deploy a set of basic demo examples. youc an opbserve teh example sites at any time with the command below.

**Observe Deployment**
<br>
Ensure you are in the correct namespace
```
kubectl get all
```

## Deploy the basic NGINX examples(s)

**Example One**

```
kubectl create -f nginx-deploy-main.yaml
```
## K8s site demo 

```
kubectl apply -f run-my-nginx.yaml
```

```
kubectl expose deployment/my-nginx
```

**All examples**
```
kubectl create -f nginx-deploy-main.yaml -f nginx-deploy-blue.yaml -f nginx-deploy-green.yaml
```

## Expose the basic example(s) as a service on port 80

**One Example**

```
kubectl expose deploy nginx-deploy-main --port 80
```

**All examples**

```
kubectl expose deploy nginx-deploy-main --port 80
kubectl expose deploy nginx-deploy-blue --port 80
kubectl expose deploy nginx-deploy-green --port 80
```
**Observe Service**

```
kubectl get all
```

### Create and ingress resource for the Basic app
```
kubectl create -f ingress-resource.yaml
```

## Temp

```
helm install nginx-ingress nginx-stable/nginx-ingress --set prometheus.create=true --set controller.kind=daemonset
```