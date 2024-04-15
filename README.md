# flask-app-k8s-rancher-demo
Flask app Dockerized for deployment. Kubernetes config for Rancher-managed clusters. Learn containerization, orchestration with Docker, Kubernetes, Rancher.Flask app Dockerized for deployment. Kubernetes config for Rancher-managed clusters. Learn containerization, orchestration with Docker, Kubernetes, Rancher.


Go to /app directory and run the following docker build command 
```
docker build -f Dockerfile -t hello-python:latest .
```

After building check if the docker image was created correctly.
```
docker image ls
```

Test the docker run using 
```
docker run -p 5001:5000 hello-python
```
## Kubectl setup
Install using following command 
```
sudo snap install kubectl --classic
```
if following error is seen:
"The connection to the server <server-name:port> was refused - did you specify the right host or port?"
Use below command to run a cluster since working locally (using minikube),

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube && rm minikube-linux-amd64
```

After installing, start minikube cluster
```
minikube start
```


## Load the docker image to minikube 
```
sudo docker save hello-python | bzip2 > hello-python.bz2 # saves the image 
sudo scp -i $(minikube ssh-key) hello-python.bz2 docker@$(minikube ip):/home/docker/hello-python.bz2 # copies it to minikube, in case this didn't work run the code on terminal

minikube ssh # ssh to minikube
docker load -i hello-python.bz2 # load the docker image
```

Finally run, 
```
kubectl apply -f deployment.yaml
```

To create a visible ip on minikube run the following: 
```
minikube service hello-python-service
```


To cleanup the deployment do the following:
```
kubectl delete deployment hello-python
minikube stop # to stop the minikube 
```




