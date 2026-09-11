# Guide

In this challenge you will be hosting your own minikube cluster and deploying a tetris application to it using Terraform and Helm.

!!! tip "Already confident with these technologies?"
    If you're confident using docker, and/or minikube then you could try deploying a different image to the one we're focusing on today. This will require editing the **Terraform** and **Helm** files in order to deploy successfully. There are no specific instructions on this but if you get stuck you can look at these [hints](hints.md#deploying-a-different-app)

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

You'll be deploying [**bsord/tetris**](https://hub.docker.com/r/bsord/tetris), a small
web version of Tetris that serves on port **80**. 

For this challenge, you'll deploy it to your minikube cluster using **Terraform** and **Helm**.

## What's in the challenge repo

You should've cloned the challenge repo in the setup on the welcome page, if not it's [here](https://github.com/WebOpsLR/UOPComputingChallenge)

```
.
├── install-tools.sh      # installs Docker, kubectl, Minikube, Helm, Terraform
├── helm/
│   └── tetris/           # Helm chart that packages the tetris Deployment + Service
└── terraform/            # Terraform config that deploys the chart via the Helm provider
```

- **Helm chart** (`helm/tetris`) describes the Kubernetes resources: a `Deployment` running `bsord/tetris` and a `NodePort` `Service` exposing it.
- **Terraform** (`terraform/`) uses the `helm_release` resource to install that chart onto your cluster. Terraform reads your kubeconfig (the `minikube` context) to know where to deploy.

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

Alternatively, port-forward it to localhost (Typically just used for debugging if a route isn't available):
```shell
kubectl port-forward -n tetris svc/tetris-tetris 8080:80
```
Then visit http://localhost:8080.

Check the deployment status at any time:
```shell
kubectl get all -n tetris
```

## Tearing it down

!!! tip
    If you're on this stage with a good amount of time remaining then attempt the extension

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

## Extension

Now you've deployed an application to Kubernetes using predefined configuration, try to deploy your own image.

There are a few options here:

- [Docker Hub](https://hub.docker.com/hardened-images/catalog) Is a great resource with thousands of free images.
- If you've containerised an application before then you can attempt to deploy that, either from docker hub or a private container repository
- If you've got a personal web application project then attempt to containerise that and push it to docker hub so you can deploy it to your cluster (Claude or your preffered assistant could be helpful if you're new to this)