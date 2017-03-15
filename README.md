# samplecode3927
Example deployment with Kubernetes and NGINX to demonstrate preserved source IP

References:
Method 1:
  Uses GCP TCP Loadbalancer
  https://kubernetes.io/docs/user-guide/load-balancer/#loss-of-client-source-ip-for-external-traffic
  
Method 2:
  Uses GCP HTTP Loadbalancer
  https://cloud.google.com/container-engine/docs/tutorials/http-balancer
  
Setup

Create GKE Cluster

gcloud container clusters create samplecode3927 --zone europe-west1-b

Method 1:

kubectl create -f samplecode3927_deployment.yaml


  
  
