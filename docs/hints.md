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

### minikube service doesn't open a browser

On WSL this is common — use the port-forward command instead:

```shell
kubectl port-forward -n <namespace> svc/<release_name>-challenge 8080:<service.port>
```

Then visit <http://localhost:8080>.

### Fixing a value and trying again

You don't need to tear anything down first. Edit `helm/challenge/values.yaml` or
`terraform/terraform.tfvars` and re-run `terraform apply` — Helm upgrades the
existing release in place.

## Extension hints

### Deploying Elasticsearch

Elasticsearch is heavier than a simple web image, so the chart's defaults won't be enough. Start by looking at what the pod is actually doing:

```shell
kubectl get pods -n <namespace>
kubectl describe pod -n <namespace> -l app.kubernetes.io/instance=<release_name>
kubectl logs -n <namespace> -l app.kubernetes.io/instance=<release_name>
```

#### The pod gets OOMKilled or keeps restarting

Elasticsearch needs far more memory than the chart's defaults. In the **describe** events you'll see **OOMKilled** or the pod restarting. Raise the limits in `values.yaml`:

```yaml
# helm/challenge/values.yaml
resources:
  requests:
    cpu: 500m
    memory: 1Gi
  limits:
    cpu: "1"
    memory: 2Gi
```

You may also need to tell Elasticsearch to run as a single node by setting the `discovery.type=single-node` environment variable.