# Build image of nginx and use it with minikube

## Prerequisites

* devbox is installed and `devbox shell` was executed.
* The minikube devenv cluster should be running locally.

```bash
task devenv-status
```

## Simple Webapp Example

Build a simple web app available to minikube, create a deployment, expose the service as type LoadBalancer and create a ingress.

```bash
minikube image build -t simple-webapp . --profile devenv
kubectl create -f simple-webapp.yaml
```

In a different terminal from the repo directory open the tunnel after executing `devbox shell`. Running the tunnel on port 80 will require sudo access and prompt for the root password.

```bash
minikube tunnel --profile devenv
```

Opening the tunnel will assign IP `127.0.0.1` as the EXTERNAL-IP to the simple-webapp service

```
devenv➜ k get svc
NAME            TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP      10.96.0.1        <none>        443/TCP        14m
simple-webapp   LoadBalancer   10.101.173.249   127.0.0.1     80:31313/TCP   13m
```

Browse the service using the hostname set in the ingress -  http://simple-webapp-127-0-0-1.nip.io

TODO: Investigate trying to get a working local tls ingress with this setup. 

Build and run the image using podman exposed on port 8080.

```bash
podman build -t simple-webapp .
podman container run -dt -p 8080:80 --name simple-webapp localhost/simple-webapp
```