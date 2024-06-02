# Install istio and run through demo

## Prerequisites

* devbox is installed and `devbox shell` was executed.
* Destroy an existing cluster if it exists macOS `task devenv-destroy` chromeOS `task devenv-destroy-chromeOS`
* Initialize a fresh large minikube cluster for this demo.

[Install istio on minikube](https://istio.io/latest/docs/setup/platform-setup/minikube/)

Istio requires at least 4 cpus and 16 gb of memory according to the documentation.

## macOS

```bash
cd ..; task devenv-status
cd ..; task devenv-init-large
```

## chromeOS

```bash
cd ..; task devenv-status
cd ..; task devenv-init-chromeOS
```

## Install istio

Task setup to follow istio getting started [here](https://istio.io/latest/docs/setup/getting-started/) for version 1.22.0

```bash
task download
task install-demo
```

After a successful installation open a tunnel in a new terminal as mentioned [here](https://istio.io/latest/docs/setup/getting-started/#determining-the-ingress-ip-and-ports)

```bash
devbox shell
minikube tunnel --profile devenv
```

From a new terminal setup the environment. 

```bash
devbox shell
export INGRESS_HOST=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].port}')
echo "$INGRESS_HOST"
echo "$INGRESS_PORT"
echo "$SECURE_INGRESS_PORT"
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
echo "$GATEWAY_URL"
echo "http://$GATEWAY_URL/productpage"
curl -I "http://$GATEWAY_URL/productpage"
```

Send some traffic to the service mesh

```bash
for i in $(seq 1 100); do curl -s -o /dev/null "http://$GATEWAY_URL/productpage"; done
```

Check the dashboard

```bash
istioctl dashboard kiali
```



