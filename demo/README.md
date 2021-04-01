# NGINX Plus Ingress Demo

Follow the directions below to deploy a set of basic demo example.

**Observe Deployment**
<br>
Ensure you are in the correct namespace
```
watch kubectl get all
```
or
```
watch kubectl get pods --all-namespaces
```

## Deploy the basic demo

**Demo**


Create the deployment below

```
kubectl apply -f deployment.yaml
```

```
kubectl apply -f service.yaml
```
Update this yaml file with your demo app DNS address.
```
kubectl apply -f ingress.yaml
```
You view the site at the link specified in the ingress.yaml