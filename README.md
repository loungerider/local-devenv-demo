# Create an ephemeral minikube local dev environment

[Install devbox](https://www.jetify.com/devbox/docs/installing_devbox/)

The following command will install the nix package manager, download all the packages listed in the `devbox.json` and place you in the devbox shell. The first execution will be time consuming because of the nix setup and package downloads.

```bash
devbox shell
```

This demo assumes that podman and minikube are not installed on your OS. go-task is used to abstract common commands.

```bash
task --list
task devenv-init
```

