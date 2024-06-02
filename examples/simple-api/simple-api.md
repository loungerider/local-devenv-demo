# Build a simple api with go

## Prerequisites

* devbox is installed and `devbox shell` was executed.
* The minikube devenv cluster should be running locally.

```bash
cd ..; task devenv-status
```

## Simple Api Example

[Go API Reference Article](https://dev.to/envitab/how-to-build-an-api-using-go-ffk)\
[Go Dev Example](https://go.dev/doc/tutorial/web-service-gin)

Run api locally

```bash
go mod init simple-api
go mod tidy
go run .
```

Build and deploy with minikube

```bash
task build
task deploy
minikube service simple-api --profile devenv
```


Build and run using podman

```bash
podman build -t simple-api .
podman container run -dt -p 8080:8080 --name simple-api localhost/simple-api
```

