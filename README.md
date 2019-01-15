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





