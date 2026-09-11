# Hints

Stuck on a step in the guide? Open a hint below.

## WSL Basic Commands

New to the terminal? WSL gives you a Linux command line inside Windows: you type
a command, press ++enter++, and it runs. These hints cover just enough to get
through the challenge.

### Opening your terminal
Open the **Windows Terminal** app (or the **Ubuntu** app) from the Start menu. You'll see a prompt ending in `$` waiting for you to type. If you're using terminal make sure you're in WSL and not powershell.

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

Start typing a name and press ++tab++ to auto-complete it.

### Finding your cloned repo
Move into the repo before running commands:

```shell
cd ~/UOPComputingChallenge   # adjust if you cloned it elsewhere
ls                           # expect: install-tools.sh, helm/, terraform/
```

!!! info "Cloned it from Windows?" 
    You can open the folder in VSCode and open a terminal inside the IDE to continue.

### Running the scripts in this challenge
```shell
chmod +x install-tools.sh   # make it executable (if required)
sudo ./install-tools.sh     # run with admin rights
```

`sudo` runs as administrator — needed to install software. This will ask for the password you created for your WSL user.

## Deploying a different app

This methodology will work with any docker image with some tweaks to the helm values. Everything lives in `helm/tetris/values.yaml` — edit that file, then re-run `terraform apply`.

??? tip "Changing the image"
    If your new image runs on port `80` like our tetris example, this is the only change needed:

    ```yaml
    # helm/tetris/values.yaml
    image:
      repository: nginxdemos/hello
      tag: latest
    ```

??? tip "Changing the external port"
    Must be in the 30000–32767 range:

    ```yaml
    # helm/tetris/values.yaml
    service:
      nodePort: 30090
    ```

??? tip "Changing container port"
    Set both to the port your app serves on:

    ```yaml
    # helm/tetris/values.yaml
    containerPort: 8080
    service:
      port: 8080
    ```

??? tip "The pod never becomes Ready (health checks)"
    The probes `GET /` on the app. If yours serves health elsewhere:

    ```yaml
    # helm/tetris/templates/deployment.yaml
    readinessProbe:
      httpGet:
        path: /health
        port: http
    ```

    Not an HTTP app? Use a `tcpSocket` probe or remove the probes. Slow to
    start? Raise `initialDelaySeconds`.


## Troubleshooting

??? tip "`terraform apply` can't reach the cluster"
    Check `minikube status` shows it running and `kubectl get nodes` works.
    Terraform uses the `minikube` context by default; override with
    `-var="kube_context=<name>"` if yours differs.

??? tip "Pod stuck in `ImagePullBackOff`"
    Check connectivity to Docker Hub:

    ```shell
    kubectl describe pod -n tetris -l app.kubernetes.io/name=tetris
    ```

??? tip "`minikube service` doesn't open a browser"
    On WSL this is common — use the port-forward command instead.
