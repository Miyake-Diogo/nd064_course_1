# Imperative demo commands 

Demos commands for kubernetes imperative methods.

I run this commands in K3D cluster at my local machine.  

# Demo1 Walktrough
First create a cluster (you can use any app, minikube, kind, k3d, etc)
```
k3d cluster create udacity-course 
```
Automatically k3d configure to use a cluster
```
kubectl config use-context k3d-udacity-course
```
To see cluster infos use:  
```
kubectl cluster-info
```
Use command below to see node for cluster
```
kubectl get nodes 
```
Use command below to see node for cluster in friendly view
```
kubectl get nodes -o wide
```
Use command below to describe infos for server
```
kubectl describe node k3d-udacity-course-server-0
```
Use command below to describe infos for server but filtering for CIDR
```
kubectl describe node localhost | grep CIDR
```
See if cluster has deploys
```
kubectl get deploy
```
See if cluster has replica set
```
kubectl get rs 
```
See if cluster has pods
```
kubectl get pods
```
Create a deploy using image created in other lesson
```
kubectl create deploy go-helloworld --image=miyakediogo/go-helloworld:v1.0.0
```
Now, see that we have a deploy
```
kubectl get deploy 
```
Now, see that we have a replica set
```
kubectl get rs 
```
Now, see that we have pods
```
kubectl get pods
```
Now we go to open port to comunicate with pod
```
kubectl port-forward po/go-helloworld-5cb897596f-ws6hj 6111:6111 
```
Go to local borwoser and write localhost:6111 to see helloworld message from app.

To edit deploy:  
```
kubectl edit deploy go-helloworld -o yaml
```
Edit in VIM version of container from 1 to 2.

After this see if pods are updated with `kubectl get rs`and `kubectl get pods`. 

Change the port forward to 6112:6112 and test in browser.

---
# Demo 2 Walktrough
```
kubectl get deploy
```

```
kubectl get pods 
```

```
$ kubectl get svc

#  NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
#  kubernetes   ClusterIP   10.43.0.1    <none>        443/TCP   7d1h
```

```
kubectl expose deploy go-helloworld --port=6112 --target-port=6112
# NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
kubernetes      ClusterIP   10.43.0.1       <none>        443/# TCP    7d1h
go-helloworld   ClusterIP   10.43.202.127   <none>        6112/TCP   12s

```
To check if we can access application we need to create a pod 

```
$ kubectl run test-$RANDOM --namespace=default --rm -it --image=alpine -- sh
```
After run a pod write line below to check if app is acessible using wget (ohter option is curl)
```
$ wget -qO 10.43.202.127:6112


# return
BusyBox v1.33.1 () multi-call binary.

Usage: wget [-cqS] [--spider] [-O FILE] [-o LOGFILE] [--header 'HEADER: VALUE'] [-Y on/off]
        [-P DIR] [-U AGENT] [-T SEC] URL...

Retrieve files via HTTP or FTP

        --spider        Only check URL existence: $? is 0 if exists
        -c              Continue retrieval of aborted transfer
        -q              Quiet
        -P DIR          Save to DIR (default .)
        -S              Show server response
        -T SEC          Network read timeout is SEC seconds
        -O FILE         Save to FILE ('-' for stdout)
        -o LOGFILE      Log messages to FILE
        -U STR          Use STR for User-Agent header
        -Y on/off       Use proxy

```

--- 
# Demo 3 Walktrough

To show configmaps
```
kubectl get cm 
```
To create configmaps
```
kubectl create cm test-cm --from-literal=color=blue
```
Show again to see cm created
```
kubectl get cm 
```
To retrieve more info about configmaps
```
kubectl describe cm test-cm
```
To get secrets
```
kubectl get secrets
```
To make a secret
```
kubectl create secret generic --from-literal=color=red
```
TO make another secret
```
kubectl create secret generic test-sec --from-literal=color=red
```
Show the secrets
```
kubectl get secrets 
```
Describe info about secret
```
kubectl describe secrets test-sec
```
To see info in yaml format
```
kubectl get secrets test-sec -o yaml
```
Note that color in describe command above return a encoded color, to see decoded use command below
```
echo "cmVk" | base64 -D
```

Show namespaces
```
kubectl get ns
```
To create namespace
```
kubectl create ns test-udacity
```
Show namespaces again!! 
```
kubectl get ns
```
Show all pods
```
kubectl get pods
```
Show all pods in some namespace
```
kubectl get pods -n test-udacity
```
