# Create an ephemeral minikube local dev environment with podman

[Install devbox](https://www.jetify.com/devbox/docs/installing_devbox/)

## macOS

[Install vfkit if using macOS](https://github.com/NixOS/nixpkgs/issues/305868)

```bash
brew tap cfergeau/crc
brew install vfkit
```

This configuration has only been tested on macOS:
* `Darwin Kernel Version 23.4.0 x86_64` and `Darwin Kernel Version 23.2.0 arm64`.
* Running a linux VM in `Virutalbox 7.0.18` or `qemu-system-x86_64` on macOS is not supported. qemu in the VM requires the kvm kernel module [issue](https://github.com/containers/podman/issues/19138).

## chromeOS

[Install libvirt based on these directions](https://richrose.dev/posts/chromeos/productivity/chromeos-minikube/)

```bash
sudo apt install -y libvirt-clients libvirt-daemon-system
```

TODO: Test on Windows and Linux running on bare metal.

## Activate devbox

The following command will install the nix package manager, download all the packages listed in the `devbox.json` and place you in the devbox shell. The first execution will be time consuming because of the nix setup and package downloads.

```bash
devbox shell
```

This demo assumes that podman and minikube are not installed on your OS. go-task is used to abstract common commands. A custom KUBECONFIG file will be generated and used for this demo. Podman is running in `rootful` mode because the minikube ingress doesn't work in `rootless` created [issue](https://github.com/kubernetes/minikube/issues/18978). Using the containerd runtime instead of cri-o with `rootful` podman as suggested [here](https://minikube.sigs.k8s.io/docs/drivers/podman/) the cri-o container runtime will error when executing `minikube image build`. 

```bash
task --list
task devenv-init
```

Using a single node minikube cluster because some examples would have issues when tunneling to the exposed service when executing `minikube service <app_name> --profile devenv` on macOS

To destroy the cluster on macOS

```bash
task devenv-destroy
```

If minikube is unresponsive, this will kill the podman and vfkit processes, force remove the podman machine and cleanup the minikube config.

```bash
task devenv-hung
```
