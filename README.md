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

Hit the NGINX service:

curl <samplecode3927-web external IP>

View the NGINX logs

kubectl get pods

e.g: samplecode3927-nginx-deployment-1935786584-r86pm

kubectl logs samplecode3927-nginx-deployment-1935786584-r86pm

<REQUESTORS IP> - - [15/Mar/2017:10:42:44 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.51.0" "-"

Cleanup

kubectl delete services samplecode3927-web

If you wish to carry on and try Method 2 you can skip this next step

kubectl delete deployments samplecode3927-nginx-deployment

Method 2:





  
  
