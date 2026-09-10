# Guide

In this challenge you will be hosting your own minikube cluster and deploying an application to it using Terraform and Helm.

## Getting Started

To complete this challenge you will need:

- Windows
    - WSL 2.0
    - Docker Desktop

- Linux
    - Docker

- Mac
    - Docker Desktop

## Prerequisites

Before running `install-tools.sh`, check what you already have. The script will
install anything missing, but confirming these first avoids surprises once the
clock is running.

### Windows

- [ ] **WSL 2** is installed and a distro setup, Ubuntu is recommended from Windows store.
- [ ] **Docker Desktop** is installed and running.
- [ ] Docker Desktop > Settings > General: *Use the WSL 2 based engine* is enabled.
- [ ] Docker Desktop > Settings > Resources > WSL Integration: your distro is enabled.
- [ ] Inside WSL, `docker info` succeeds.

### Linux

- [ ] A supported package manager: `apt`, `dnf`, or `yum`.
- [ ] Permission to run `sudo` (needed to install Docker and add your user to
      the `docker` group).
- [ ] After Docker install, remember you must log out/in for the `docker`
      group membership to take effect.

### Mac

- [ ] **Homebrew** installed (the script installs it if missing).
- [ ] **Docker Desktop** installed and launched at least once so the engine is running.

Once every box is ticked and `docker info` works, you're ready to run
`install-tools.sh`.

## Install required tools

Use the provided `install-tools.sh` script to ensure your machine has everything required to complete this.

This verifies the installation of:
- **Docker**
- **kubectl**
- **Minikube**
- **Helm**
- **Terraform**

### Windows (via WSL)

Windows users run everything inside WSL.

1. Install **Docker Desktop** and enable WSL 2 integration:
   - Docker Desktop > Settings > General: enable *Use the WSL 2 based engine*.
   - Docker Desktop > Settings > Resources > WSL Integration: enable your distro
     (e.g. Ubuntu).
2. Open your WSL distro and run:
   ```shell
   ./install-tools.sh
   ```

### Linux

Open a terminal and run:
```shell
./install-tools.sh
```

!!! tip
    you may need to open a new terminal for changes to take effect.

### Mac

Open a terminal and run:
```shell
./install-tools.sh
```

The script uses Homebrew (installing it first if needed) and installs Docker
Desktop, kubectl, Minikube, Helm, and Terraform. Launch Docker Desktop once to
start the engine.

## Starting your cluster

Once the tools are installed and Docker is running:
```shell
minikube start
```

This creates a local single-node Kubernetes cluster and sets your kubeconfig
context to `minikube`. Confirm it's up:
```shell
kubectl get nodes
```

## The application

You'll deploy [**bsord/tetris**](https://hub.docker.com/r/bsord/tetris), a small
web version of Tetris that serves on port **80**.

If you just want to see it run in plain Docker (no Kubernetes), you can do:
```shell
docker run -d -p 80:80 --name tetris bsord/tetris
```
Then open http://localhost. Stop and remove it again with:
```shell
docker rm -f tetris
```

For this challenge, though, you deploy it to your minikube cluster using
**Terraform** and **Helm**.

## What's in this repo

```
.
├── install-tools.sh      # installs Docker, kubectl, Minikube, Helm, Terraform
├── helm/
│   └── tetris/           # Helm chart that packages the tetris Deployment + Service
└── terraform/            # Terraform config that deploys the chart via the Helm provider
```

- **Helm chart** (`helm/tetris`) describes the Kubernetes resources: a
  `Deployment` running `bsord/tetris` and a `NodePort` `Service` exposing it.
- **Terraform** (`terraform/`) uses the `helm_release` resource to install that
  chart onto your cluster. Terraform reads your kubeconfig (the `minikube`
  context) to know where to deploy.

## Deploying with Terraform and Helm

Make sure `minikube` is running first (`minikube status`).

1. Move into the Terraform directory:
   ```shell
   cd terraform
   ```
2. Initialise Terraform (downloads the Helm provider):
   ```shell
   terraform init
   ```
3. Preview what will be created:
   ```shell
   terraform plan
   ```
4. Apply to deploy the chart:
   ```shell
   terraform apply
   ```
   Type `yes` when prompted. Terraform installs the Helm release and waits for
   the pod to become ready.

When it finishes, Terraform prints outputs including the command to open the app.

## Accessing the app

The chart exposes the app as a `NodePort` service. The easiest way to open it
on minikube:
```shell
minikube service tetris-tetris -n tetris
```
This opens your browser to the running game. (The service is named
`tetris-tetris` because Helm prefixes resources with the release name `tetris`.)

Alternatively, port-forward it to localhost:
```shell
kubectl port-forward -n tetris svc/tetris-tetris 8080:80
```
Then visit http://localhost:8080.

Check the deployment status at any time:
```shell
kubectl get all -n tetris
```

## Tearing it down

Remove everything Terraform created (the Helm release, and the namespace):
```shell
cd terraform
terraform destroy
```
Type `yes` when prompted.

To shut down or delete the whole cluster:
```shell
minikube stop      # stop the cluster (keeps state)
minikube delete    # delete the cluster entirely
```

## Customising the deployment

The Terraform config exposes a few variables (see `terraform/variables.tf`).
For example, to expose the app on a different NodePort:
```shell
terraform apply -var="node_port=30090"
```
Or to pin a specific image tag:
```shell
terraform apply -var="image_tag=latest"
```

You can also tweak chart defaults directly in `helm/tetris/values.yaml`
(replica count, resource limits, service type, etc.).

## Troubleshooting

- **`terraform apply` can't reach the cluster** — ensure `minikube status`
  shows it running and that `kubectl get nodes` works. Terraform uses the
  `minikube` kubeconfig context by default; override it with
  `-var="kube_context=<name>"` if yours differs.
- **Pod stuck in `ImagePullBackOff`** — check connectivity to Docker Hub with
  `kubectl describe pod -n tetris -l app.kubernetes.io/name=tetris`.
- **`minikube service` doesn't open a browser** (e.g. on WSL) — use the
  port-forward command above instead.