# Build image of nginx and use it with minikube

## Prerequisite

The minikube devenv cluster should be running locally

```bash
task devenv-status
```

## Simple Webapp Example

Build a simple web app available to minikube, create a deployment, expose the service as type LoadBalancer and create a tunnel to browse the service.

```bash
minikube image build -t simple-webapp . --profile devenv
kubectl create -f simple-webapp.yaml
kubectl expose deployment simple-webapp --type=LoadBalancer --port=80 --target-port=80
minikube service simple-webapp --profile devenv
```

Build and run the image using podman exposed on port 8080.

```bash
podman build -t simple-webapp .
podman container run -dt -p 8080:80 --name simple-webapp localhost/simple-webapp
```