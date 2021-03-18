# NGINX Plus Ingress Demo

Follow the directions below to deploy a set of basic demo applications.

### Deploy the basic NGINX app
```
kubectl create -f nginx-deploy-main.yaml
```
### Expose the basic app as a service on port 80
```
kubectl expose deploy nginx-deploy-main --port 80
```
### Create and ingress resource for the Basic app
```
kubectl create -f ingress-resource-nginx-1.yaml
```