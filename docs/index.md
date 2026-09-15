# Welcome

Welcome to the DevOps Computing Challenge.

This challenge walks you through a hands-on DevOps workflow using Docker, Kubernetes (via Minikube), Helm, and Terraform. Before you dive in, make sure your machine is set up with the required tools.

[Setup](setup.md){ .md-button .md-button--primary } [Guide](guide.md){ .md-button } [Hints](hints.md){ .md-button }

## Getting Started

1. **[Set up your machine](setup.md)** — install the required tools and clone
   the challenge repository.
2. **[Follow the guide](guide.md)** — work through the challenge tasks.
3. **[Check the hints](hints.md)** — if you get stuck along the way.

## Industry Relevance

At HM Land Registry (HMLR), we use technologies such as [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift/container-platform) and other cloud-native tooling to build, deploy, and operate applications throughout the software development lifecycle (SDLC). This challenge provides a simplified introduction to the processes and technologies involved in taking an application from development through to deployment and operation.

The tools you use here reflect common practices used across modern DevOps teams:

- **Docker** containerises applications, ensuring they behave consistently across different environments.
- **Kubernetes** orchestrates those containers, handling tasks such as scaling, self-healing, and application deployment. In this challenge, you will use Minikube as a local Kubernetes environment.
- **Helm** packages and templates Kubernetes resources, allowing applications to be deployed consistently while supporting environment-specific configuration.
- **Terraform** enables infrastructure to be defined and managed as code, making environments reproducible, version-controlled, and easier to maintain.

While this challenge uses a local Kubernetes cluster and a simplified deployment workflow, the concepts are the same ones used across the industry to manage applications and infrastructure at scale. Infrastructure as Code, containerisation, orchestration, and automated deployments form the foundation of modern cloud platforms and DevOps practices.

By the end of this challenge, you will have followed the core stages involved in deploying and running an application using industry-standard tools, gaining practical experience with technologies commonly used throughout the software development lifecycle.
## Useful Resources

Here are some links to resources that will be useful for this challenge and for gaining a wider understanding of DevOps.

These links are for resources relevant to the challenge:

- [Minikube documentation](https://minikube.sigs.k8s.io/docs/)
- [Docker documentation](https://docs.docker.com/)
- [Helm documentation](https://helm.sh/docs/)
- [Terraform documentation](https://developer.hashicorp.com/terraform/docs)
- [WSL documentation](https://wsl.dev/)

These links are resources for a wider understanding of DevOps:

- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Overview of CI/CD](https://www.redhat.com/en/topics/devops/what-is-ci-cd)
- [DevOps Principles](https://www.atlassian.com/devops/what-is-devops)
- [OpenSSH Manual](https://www.openssh.org/manual.html)
- [GitHub Actions](https://github.com/features/actions)
- [GitLab Pipelines](https://docs.gitlab.com/ci/pipelines/)