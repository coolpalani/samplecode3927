# samplecode3927
Example deployment with Kubernetes and NGINX to demonstrate preserved source IP

References:
Method 1:
  Uses GCP TCP Loadbalancer
  https://kubernetes.io/docs/user-guide/load-balancer/#annotation-to-modify-the-loadbalancer-behavior-for-preservation-of-source-ip
  
  Results in NGINX log Format:
  
  
Method 2:
  Uses GCP HTTP Loadbalancer
  https://cloud.google.com/container-engine/docs/tutorials/http-balancer
  
  Results in NGINX log Format:
  
  
Setup

Create GKE Cluster

gcloud container clusters create samplecode3927 --zone europe-west1-b

Deploy NGINX

kubectl create -f deployment.yaml

Method 1:

Deploy "web" service with Loadbalancer

kubectl create -f service.yaml

Get external IP of "web" service

kubectl get services

This deployment only creates one Pod so all traffic hit a single NGINX service that we can monitor the logs of.
Get the pod name:




  
  
