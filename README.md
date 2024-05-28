# Create an ephemeral minikube local dev environment with podman

[Install devbox](https://www.jetify.com/devbox/docs/installing_devbox/)\
[Install vfkit if using macOS](https://github.com/NixOS/nixpkgs/issues/305868)

This configuration has only been tested on macOS `Darwin Kernel Version 23.4.0 x86_64`

```bash
brew tap cfergeau/crc
brew install vfkit
```

The following command will install the nix package manager, download all the packages listed in the `devbox.json` and place you in the devbox shell. The first execution will be time consuming because of the nix setup and package downloads.

```bash
devbox shell
```

This demo assumes that podman and minikube are not installed on your OS. go-task is used to abstract common commands. A custom KUBECONFIG file will be generated and used for this demo. Podman in running in `rootful` mode becaue the minikube ingress doesn't work in `rootless`. Using the containerd runtime instead of cri-o with `rootful` podman as suggested [here](https://minikube.sigs.k8s.io/docs/drivers/podman/) because the cri-o container runtime will error when executing `minikube image build`. 

```bash
task --list
task devenv-init
```

Using a single node minikube cluster because some examples would have issues when tunneling to the exposed service when executing `minikube service <app_name> --profile devenv` on macOS
