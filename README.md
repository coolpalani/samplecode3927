# samplecode3927
Example deployment with Kubernetes and NGINX to demonstrate preserved source IP

Works with both NLB and GCLB

Packet walk and detailed explanation here

* https://www.youtube.com/watch?v=y2bhV81MfKQ&t=2080s

---

### You will need a GKE cluster to run these examples

```
gcloud container clusters create samplecode3927 --zone europe-west1-b
```

---

## Method 1: Using Network Load Balancer - NLB (TCP)

Deploy NGINX

```
kubectl create -f deployment.yaml
```

Deploy service with Loadbalancer

```
kubectl create -f service-lb.yaml
```

Get external IP of "samplecode3927-web" service

```
kubectl get services
```

Hit the NGINX service:

```
curl <samplecode3927-web external IP>
```

To view the NGINX logs, first get the Pod name

```
kubectl get pods -l app=samplecode3927-nginx
```

e.g: samplecode3927-nginx-deployment-1935786584-r86pm

Then get the logs for that Pod

```
kubectl logs samplecode3927-nginx-deployment-1935786584-r86pm
```

Output should look like:

```
<REMOTE_IP> - - [15/Mar/2017:10:42:44 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.51.0" "-"
```

#### Cleanup

```
kubectl delete services samplecode3927-web-lb
```
```
kubectl delete deployments samplecode3927-nginx-deployment
```

---

## Method 2:  Using Google Cloud Load Balancer - GCLB (HTTP/HTTPS)  

Deploy NGINX

```
kubectl create -f deployment.yaml
```

Deploy service exposing on NodePorts
Note:   Local Only annotation is optional in this manifest with regards to preserving source IP.
        Source IP will be carried in X-Forward-For field regardless.

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

Note: Even after the IP is assigned, it can still take a few minutes for the full path to be setup.


Hit the NGINX service

```
curl <samplecode3927-web external IP>
```

If you do not get the welcome to NGINX page, wait 30 seconds and try again


To view the NGINX logs, first get the Pod name

```
kubectl get pods -l app=samplecode3927-nginx
```

e.g: samplecode3927-nginx-deployment-1935786584-r86pm

Then get the logs for that Pod

```
kubectl logs samplecode3927-nginx-deployment-1935786584-r86pm
```

#### Output should look like

```
<Google Internal IP> - - [16/Mar/2017:14:51:51 +0000] "GET / HTTP/1.1" 200 612 "-" "curl/7.51.0" "<Remote IP>"
```

#### Cleanup

```
kubectl delete services samplecode3927-web-np
```

```
kubectl delete ingress samplecode3927-ingress
```

```
kubectl delete deployments samplecode3927-nginx-deployment
```
