# Build a simple api with go

[Go API Reference Article](https://dev.to/envitab/how-to-build-an-api-using-go-ffk)
[Go Dev Example](https://go.dev/doc/tutorial/web-service-gin)

Run api locally

```bash
go mod init simple-api
go mod tidy
go run .
```

Build and deploy with minikube

```bash
minikube image build -t simple-api . --profile devenv
kubectl create -f simple-api.yaml
kubectl expose deployment simple-api --type=LoadBalancer --port=80 --target-port=8080
minikube service simple-api --profile devenv
```


Build and run using podman

```bash
podman build -t simple-api .
podman container run -dt -p 8080:8080 --name simple-api localhost/simple-api
```

