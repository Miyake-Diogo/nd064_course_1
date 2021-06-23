# Declarative demo commands 

Demos commands for kubernetes declarative methods.

I run this commands in K3D cluster at my local machine.  

# Demo Walkthrough
Show Namespaces
```
kubectl get ns
```
Create namespace demo
```
kubectl create ns demo --dry-run=client -o yaml
```
create yaml for namespace
```
kubectl create ns demo --dry-run=client -o yaml > namespace.yaml
```
Create namespace by apply command
```
kubectl apply -f namespace.yaml
```
Show namespaces Again!!!
```
kubectl get ns
```
Create Deployment for busybox
```
kubectl create deploy busybox --image=busybox -r=5 -n demo --dry-run=client -o yaml
```
Create yaml Deployment for busybox
```
kubectl create deploy busybox --image=busybox -r=5 -n demo --dry-run=client -o yaml > deployment.yaml
```
Create yaml Deployment for busybox by apply command
```
kubectl apply -f deployment.yaml
```
Show deploys by namespace
```
kubectl get deploy -n demo
```
Show pods by namespace
```
kubectl get pods -n demo
```