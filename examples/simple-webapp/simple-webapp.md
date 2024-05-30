# Build image of nginx and use it with minikube

## Prerequisites

* devbox is installed and `devbox shell` was executed.
* The minikube devenv cluster should be running locally using `task devenv-init` which includes the nginx ingress.

```bash
task devenv-status
```

## Simple Webapp Example

Build a simple web app available to minikube, create a deployment, expose the service as type LoadBalancer and create a ingress.

```bash
minikube image build -t simple-webapp . --profile devenv
kubectl create -f simple-webapp.yaml
```

In a new terminal session from your local repo directory execute `devbox shell`. Then open a tunnel on port 80 and 443 which requires sudo access and will prompt for the root password.

```bash
minikube tunnel --profile devenv
```

```
✅  Tunnel successfully started

📌  NOTE: Please do not close this terminal as this process must stay alive for the tunnel to be accessible ...

❗  The service/ingress my-ingress requires privileged ports to be exposed: [80 443]
🔑  sudo permission will be asked for it.
🏃  Starting tunnel for service my-ingress.
Password:
```

Browse the service using the hostname set in the ingress -  http://simple-webapp-127-0-0-1.nip.io or https://simple-webapp-127-0-0-1.nip.io

TODO: Investigate creating a valid local tls ingress.

Build and run the image using podman exposed on port 8080.

```bash
podman build -t simple-webapp .
podman container run -dt -p 8080:80 --name simple-webapp localhost/simple-webapp
```