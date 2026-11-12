# Hints

Stuck on a step in the guide? Find the matching section below.

## WSL Basic Commands

Windows Subsystem for Linux gives you a Linux command line inside Windows: you type a command, press ++enter++, and it runs. These cover just enough to get
through the challenge.

### Getting around the file system

Files live in directories. These move you around:

| Command | What it does | Example |
| --- | --- | --- |
| `pwd` | Print working directory — the folder you're in | `pwd` |
| `ls` | List files and folders here | `ls` |
| `ls -la` | List everything, including hidden files, with details | `ls -la` |
| `cd <folder>` | Change directory — move into a folder | `cd terraform` |
| `cd ..` | Move up one folder | `cd ..` |
| `cd ~` | Go to your home folder | `cd ~` |
| `clear` | Clear the screen | `clear` |
| `history` | See previously used commands | `history` |

Start typing a name and press ++tab++ to auto-complete it, and press ++up++ to bring back the previous command.

## Challenge Hints

!!! info "Names in these commands"
    Commands below use **<release_name>** and **<namespace>** — replace them with the values you set in `terraform/terraform.tfvars`. The Service is named **<release_name>-challenge**.

### terraform apply can't reach the cluster

Check `minikube status` shows everything **Running** and `kubectl get nodes`
works. If minikube isn't running, start it with `minikube start`. Terraform
uses the `minikube` context by default.

### terraform apply hangs, then fails with a timeout

Terraform waits up to 5 minutes for the pod to become Ready before it finishes,
so a pod that never starts looks like a slow apply that eventually times out
rather than failing straight away. Diagnose with:

```shell
kubectl get pods -n <namespace>
kubectl describe pod -n <namespace> -l app.kubernetes.io/instance=<release_name>
```

The usual causes are a wrong `containerPort` or the image needing more memory (**OOMKilled** in events).

### Pod stuck in ImagePullBackOff

Usually a wrong image name or tag in `values.yaml`, or no connectivity to Docker
Hub. Check what's happening:

```shell
kubectl describe pod -n <namespace> -l app.kubernetes.io/instance=<release_name>
```

### nodePort error: port is already allocated

Each `nodePort` can only be used once on the cluster. Pick a different number in the 30000-32767 range in `values.yaml` and re-run `terraform apply`.

### The pod never becomes Ready (health checks)

The chart's probes request `GET /` on the container port. If your app serves its health check somewhere else, point the probes at it in `helm/challenge/templates/deployment.yaml`:

```yaml
# helm/challenge/templates/deployment.yaml
readinessProbe:
  httpGet:
    path: /health
    port: http
```

Not an HTTP app? Use a `tcpSocket` probe or remove the probes. as the pod will start without them

### Fixing a value and trying again

You don't need to tear anything down first. Edit `helm/challenge/values.yaml` or
`terraform/terraform.tfvars` and re-run `terraform apply` — Helm upgrades the
existing release in place.

## Extension hints

### Which files do I edit?

- To make changes to your application's **image, ports, or memory limits** you will need to edit `helm/challenge/values.yaml` 
- To make changes to the **liveness and readiness probes** you will need to edit `helm/challenge/templates/deployment.yaml`

After editing either file, re-run `terraform apply`, Helm upgrades the release in place, then re-check with `kubectl get pods -n <namespace>`. No need to tear anything down.

#### The pod keeps restarting / CrashLoopBackOff with failing probes

If the events mention **Liveness probe failed** or **Readiness probe failed**, Kubernetes thinks the container is unhealthy and is restarting it. The probes check the **container port**, so if that port is wrong they'll fail every time.

Which port does Jenkins actually listen on inside the container? It's **not** the one the tetris image used, the [Jenkins image docs](https://hub.docker.com/r/jenkins/jenkins) will tell you. Once you know it, make sure the port values in `values.yaml` match the port Jenkins really serves on.

#### The pod gets OOMKilled, or the events mention memory

Jenkins needs far more memory than the chart's defaults allow for a tiny web app. If the **describe** events show **OOMKilled**, or the container keeps being killed and restarted, your application is running out of memory. Look at the `resources` section in `values.yaml` and raise the memory (and if needed CPU) limits until Jenkins has enough room to start, you may need to experiment with the numbers.