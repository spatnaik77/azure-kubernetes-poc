# azure-kubernetes-poc
# Introduction
In this POC, We will do the following:
  1. Create an AKS cluster in Azure
  2. Deploy 2 simple services, helloworld and helloindia 
  3. Create an nginx ingress controller which will route the traffic based on path, to the respective services
# Architecture
![aks-poc-architecture](https://github.com/spatnaik77/azure-kubernetes-poc/blob/master/diagrams/aks-poc-architecture.png)
# helloworld and helloindia services
We will use the following two docker images for our POC.
https://cloud.docker.com/u/spatnaik77/repository/docker/spatnaik77/helloworld
https://cloud.docker.com/u/spatnaik77/repository/docker/spatnaik77/helloindia
They are very simple rest APIs. helloworld returns "hello world" and helloindia returns "hello india"

# Getting started with the POC

## Create an aks cluster 
Create an aks cluster from azure portal

## Use Azure cloud shell to interact with aks cluster
Login to aks cluster using the following command:<br>
<b><i>az aks get-credentials --resource-group sidd-aks-poc-rg --name sidd-aks-poc-cluster</b></i>

<b><i>kubectl get nodes</b></i> : Shows the node details of the cluster <br>
NAME                       STATUS    ROLES     AGE       VERSION <br>
aks-agentpool-32009910-0   Ready     agent     3h        v1.11.5 <br>
aks-agentpool-32009910-1   Ready     agent     3h        v1.11.5 <br>
aks-agentpool-32009910-2   Ready     agent     3h        v1.11.5 <br>

## Working with aks configuration files
Following configuration files will be used:
* helloindia-deployment.yaml - Deployment for helloindia 
* helloindia-service.yaml - Load balancer for helloindia service
* helloworld-deployment.yaml -  Deployment for helloworld
* helloworld-service.yaml - Load balancer for helloworld service
* ingress.yaml - defines the routing rules for the ingress controller
Upload all the config files in the cloud shell

## Create the deployments & load balancer service for helloworld and helloindia
* <b><i>kubectl apply -f helloworld-deployment.yaml</b></i>
* <b><i>kubectl apply -f helloworld-service.yaml</b></i>
* <b><i>kubectl apply -f helloindia-deployment.yaml</b></i>
* <b><i>kubectl apply -f helloindia-service.yaml</b></i>

## Verify the Pods & Load balancer services
* <b><i>kubectl get pods -o wide</b></i> : Shows you total 6 pods (3 helloworld and 3 helloindia pods)
* <b><i>kubectl get service helloworld</b></i> : Shows you details about the load balancer for helloworld pods 
* <b><i>kubectl get service helloindia</b></i> : Shows you details about the load balancer for helloindia pods <br>
Sample output:<br>
NAME         TYPE           CLUSTER-IP   EXTERNAL-IP      PORT(S)        AGE   <br>
helloworld   LoadBalancer   10.0.192.9   40.122.113.115   80:30442/TCP   4m   <br>
siddharth@Azure:~$ kubectl get service helloindia <br>
NAME         TYPE           CLUSTER-IP   EXTERNAL-IP     PORT(S)        AGE <br>
helloindia   LoadBalancer   10.0.57.94   40.122.49.235   80:31822/TCP   4m <br>

## Create nginx ingress controller
* <b><i>helm init</b></i>  
* <b><i>helm install stable/nginx-ingress --namespace kube-system --set controller.replicaCount=2 --set rbac.create=false</b></i>
* Verify the ingress controller just created <b><i>kubectl get service -l app=nginx-ingress --namespace kube-system</b></i>
Sample output <br>
NAME                                        TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)                      AGE<br>
wistful-pig-nginx-ingress-controller        LoadBalancer   10.0.237.242   40.122.106.243   80:32173/TCP,443:30315/TCP   1m<br>
wistful-pig-nginx-ingress-default-backend   ClusterIP      10.0.161.4     <none>           80/TCP                       1m<br>

* try accessing the external ip: http://40.122.106.243 - You should see this message - "default backend 404"

## Create nginx ingress route for routing the traffic to helloworld & helloindia service based on their path
<b><i>kubectl apply -f ingress.yaml</b></i>

## Test the application:
* http://40.122.106.243/helloworld should return "Hello World"
* http://40.122.106.243/helloindia should return "Hello India"

## Kubernetes Dashboard
<b><i>az aks browse --resource-group sidd-aks-poc-rg --name sidd-aks-poc-cluster</b></i>





