# Welcome

Welcome to the WebOps Computing Challenge

[Guide](guide.md){ .md-button } [Hints](hints.md){ .md-button }

## Before you Start

### Software Requirements

To complete this challenge you will need:

#### Windows

- **WSL 2** installed and a distro setup, Ubuntu is recommended from Windows store.
- **Docker Desktop** is installed and running.
- Docker Desktop > Settings > General: *Use the WSL 2 based engine* is enabled.
- Docker Desktop > Settings > Resources > WSL Integration: your distro is enabled.
- Inside WSL, `docker info` succeeds.

#### Linux

- A supported package manager: `apt`, `dnf`, or `yum`.
- Permission to run `sudo`.
- After Docker install, remember you must log out/in for the `docker` group membership to take effect.

#### Mac

- **Homebrew** installed (the script installs it if missing).
- **Docker Desktop** installed and launched at least once so the engine is running.

### Cloning the Repository

You will need the [UOPComputingChallenge](https://github.com/WebOpsLR/UOPComputingChallenge) cloned locally to complete this challenge. This repository contains the setup script and template files you will need for a streamlined experience.

We recommend cloning with ssh through git if you have this setup:
```
git clone git@github.com:WebOpsLR/UOPComputingChallenge.git
```
If you're not familiar with using git you can clone it through the UI or your preferred means

!!! tip
    If you don't have any way of authenticating with GitHub then download the files as a `.zip` and extract into a folder of your choice

### Install required tools

Use the provided `install-tools.sh` script to ensure your machine has everything required to complete this.
This verifies the installation of:

- **Docker**
- **kubectl**
- **Minikube**
- **Helm**
- **Terraform**

!!! info
    The script has been pushed to git as an executable but if it doesn't work on your machine run `chmod +x install-tools.sh`

!!! warning "Run with sudo on Linux and WSL"
    On **Linux** and **Windows (WSL)** the script **must be run as root with `sudo`**.
    It installs system packages and writes to `/usr/local/bin`, which require
    root. Running as root also lets the installs run **in parallel** without
    stopping for a password prompt, so it finishes much faster.

    On **macOS** do the opposite: run it **without** `sudo`. Homebrew refuses to
    run as root, and the script will stop you if you try.

#### Windows (via WSL)

Windows users run everything inside WSL.

!!! tip "New to the terminal?"
    If you haven't used a command line before, read
    [WSL Basic Commands](hints.md#wsl-basic-commands) first — it covers moving
    between folders, running scripts, and using `sudo`, which is all you need
    for this challenge.

1. Install **Docker Desktop** and enable WSL 2 integration:
   - Docker Desktop > Settings > General: enable *Use the WSL 2 based engine*.
   - Docker Desktop > Settings > Resources > WSL Integration: enable your distro.
2. Open your WSL distro and run:
   ```shell
   sudo ./install-tools.sh
   ```

#### Linux

Open a terminal and run:
```shell
sudo ./install-tools.sh
```

!!! tip
    you may need to open a new terminal for changes to take effect.

#### Mac

Open a terminal and run:
```shell
./install-tools.sh
```

The script uses Homebrew and installs Docker Desktop, kubectl, Minikube, Helm, and Terraform. Launch Docker Desktop once to start the engine.
