# Spring Boot deploy to K8
In this lab we will learn to deploy the docker image with the reservation-service created [earlier](../boot-docker-demo) on to local K8s.

## Requirements  
1. Java 8+ JDK Installed  
1. Install Docker for Desktop for your platform from <https://www.docker.com/products/docker-desktop>  
1. Enable Kubernetes in Docker Desktop.
1. Donwnload and install kubectl cli for Kubernetes.  

## Confirm Kubernetes setup

1. Set kubectl to use docker desktop
```bash
kubectl config use-context docker-for-desktop
```   

1. Confirm Kubernetes and kubectl are setup correctly by running.
```bash
kubectl cluster-info
```   

You should see something like this.
```bash
Kubernetes master is running at https://localhost:6443
KubeDNS is running at https://localhost:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
```  

## Create pod.yml and service.yml files      

1. Create a text file called `pod.yml` in the `reservation-service` project directory. Copy the following text to it  

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    app: reservation-app
    deployment: reservation-service-deployment
  name: reservation-app
spec:
  containers:
  - image: foo/myfirst-boot-on-docker:latest
    name: reservation-backend
    ports:
    - containerPort: 8080
      protocol: TCP
```

Make sure the `image` matches the image tag that you specified in the [earlier](../boot-docker-demo) step. This tells Kubernetes to create a container using the docker image. The service is not exposed yet as container ports are not exposed. We need to create a service.yml for that.

1. Create a text file called `service.yml` in the `reservation-service` project directory. Copy the following text to it   

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: reservation-service
    deployment: reservation-service-deployment
  name: reservation-service
spec:
  ports:
  - port: 8080
    name: reservation-api
  selector:
    app: reservation-app
  type: LoadBalancer
```  
This file tells Kubernetes to make the container available on a port 8080 on the local machine.

1. Run the following to have Kubernetes deploy our container in a pod and expose it via port 8080.   
```bash
kubectl create -f pod.yml -f service.yml
```

1. Let's save some data to the server using curl commands
```bash
curl --request POST \
  --url http://uniquename-service.your-cf-domain.com/reservations \
  --header 'content-type: application/json' \
  --data '[{
	"id": null,
	"name": "Milan",
	"status": "booked"
}]
'
```
1. Confirm data is saved
```bash
curl --request GET \
  --url http://uniquename-service.your-cf-domain.com/reservations
```

1. Cleanup by stopping the pod.
```bash
kubectl delete pods reservation-app
```
