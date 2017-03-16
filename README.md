# samplecode3927
Example deployment with Kubernetes and NGINX to demonstrate preserved source IP

## Method 1:
  * Uses GCP TCP Loadbalancer
  * https://kubernetes.io/docs/user-guide/load-balancer/#annotation-to-modify-the-loadbalancer-behavior-for-preservation-of-source-ip
  

## Method 2:
  * Uses GCP HTTP Loadbalancer
  * https://cloud.google.com/container-engine/docs/tutorials/http-balancer
  
---

## Setup

Create GKE Cluster

```
gcloud container clusters create samplecode3927 --zone europe-west1-b
```

Deploy NGINX

```
kubectl create -f deployment.yaml
```

---

### Method 1

Deploy "web" service with Loadbalancer

```
kubectl create -f service-lb.yaml
```

Get external IP of "web" service

```
kubectl get services
```

Hit the NGINX service:

```
curl <samplecode3927-web external IP>
```

View the NGINX logs

```
kubectl get pods
```

e.g: samplecode3927-nginx-deployment-1935786584-r86pm

```
kubectl logs samplecode3927-nginx-deployment-1935786584-r86pm
```

Output should look like:

```
<REQUESTORS IP> - - [15/Mar/2017:10:42:44 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.51.0" "-"
```

#### Cleanup

```
kubectl delete services samplecode3927-web
```

If you wish to carry on and try Method 2 you can skip this next step

```
kubectl delete deployments samplecode3927-nginx-deployment
```

### Method 2

Expose the deployment on NodePort

```
kubectl create -f service-nodeport.yaml
```

Create ingress object

```
kubectl create -f ingress.yaml
```

Wait for a public IP to be assigned

```
kubectl get ingress samplecode3927-ingress --watch
```

Even after the IP is assigned, it can take a few minutes for the full path to be setup.

Hit the NGINX service

```
curl <samplecode3927-web external IP>
```

If you do not get the welcome to NGINX page, wait 30 seconds and try again

View the NGINX logs

```
kubectl get pods
```

e.g: samplecode3927-nginx-deployment-1935786584-r86pm

```
kubectl logs samplecode3927-nginx-deployment-1935786584-r86pm
```

#### Output should look like

```
<Kubernetes IP> - - [15/Mar/2017:11:22:55 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.51.0" "<REQUESTORS IP>, <"
``` 
  
