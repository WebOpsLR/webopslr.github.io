# Welcome

Welcome to the WebOps Computing Challenge.

This challenge walks you through a hands-on DevOps workflow using Docker, Kubernetes (via Minikube), Helm, and Terraform. Before you dive in, make sure your machine is set up with the required tools.

[Setup](setup.md){ .md-button .md-button--primary } [Guide](guide.md){ .md-button } [Hints](hints.md){ .md-button }

## Getting Started

1. **[Set up your machine](setup.md)** — install the required tools and clone
   the challenge repository.
2. **[Follow the guide](guide.md)** — work through the challenge tasks.
3. **[Check the hints](hints.md)** — if you get stuck along the way.

## Industry Relevance

At HM Land Registry (HMLR) we deploy almost our entire application estate on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform) (Enterprise Kubernetes). This challenge is a simplified overview of how we build, promote, and run our applications — from local development, through lower (test and staging) environments, and into production.

The tools you use here map directly into our application development lifecycle:

- **Docker** containerises our applications, ensuring consistent behaviour across environments ([benefits of containerisation](https://www.ibm.com/think/insights/the-benefits-of-containerization-and-what-it-means-for-you)).
- **Kubernetes** orchestrates those containers, handling scaling, self-healing, and rollouts across our estate. You will use Minikube as a local stand-in for the production clusters we operate.
- **Helm** packages and templates our Kubernetes deployments, meaning we can easily set environment specific values files meaning the application will be deployed consistently across environments.
- **Terraform** lets us manage infrastructure as code, so environments are replicable and version-controlled. This also allows us to deploy to multiple environments with a single code block and enables recovery in a distaster situation.

Terraform is one of the most fundamental tools we utilise, in this challenge you're just using it to deploy your application to a Minikube cluster. But we use it to deploy our whole cloud infrastructure to AWS. A small sample of the things we deploy using Terraform include our cloud OpenShift deployment, PostgreSQL databases, Redis elasticaches and VPCs which are used by our various services.

By the end of this challenge you will have followed the same core path an application takes on its way to production at HMLR, using the same industry-standard tooling we rely on every day.

## Useful Resources

Here are some links to resources that will be useful for this challenge and for gaining a wider understanding of DevOps.

These links are for resources relevant to the challenge:

- [Minikube documentation](https://minikube.sigs.k8s.io/docs/)
- [Docker documentation](https://docs.docker.com/)
- [Helm documentation](https://helm.sh/docs/)
- [Terraform documentation](https://developer.hashicorp.com/terraform/docs)
- [WSL documentation](https://wsl.dev/)
- [Git handbook](https://www.freecodecamp.org/news/the-essential-git-handbook-a1cf77ed11b5/)

These links are resources for a wider understanding of DevOps:

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Overview of CI/CD](https://www.redhat.com/en/topics/devops/what-is-ci-cd)
- [DevOps Principles](https://www.atlassian.com/devops/what-is-devops)
- [AWS Tutorials](https://aws.amazon.com/getting-started/hands-on/)
- [GitHub Actions (GitHub CI/CD)](https://github.com/features/actions)
- [Docker Container Repositories](https://docs.docker.com/docker-hub/repos/)
- [Private Container Repositories](https://www.redhat.com/en/blog/simple-container-registry)
- [OpenSSH Manual](https://www.openssh.org/manual.html)