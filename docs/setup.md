# Setup

Before starting the challenge, get your machine ready with the steps below.
Doing this in advance means you can focus on the tasks and finish within the
time limit.

Don't worry if some of the words are new — each section explains what to do
step by step. Follow the section for **your** operating system (Windows, Linux,
or Mac).

!!! info "A few terms you'll see"
    - **Terminal** (or *command line*): a window where you type commands instead
      of clicking buttons.
    - **WSL** (*Windows Subsystem for Linux*): a feature that lets Windows run
      Linux, which this challenge needs.
    - **Linux distribution** (or *distribution*): a version of Linux, such as
      **Ubuntu**. Think of it like choosing a flavour of Linux.
    - **Docker Desktop**: an app that runs the containers used in the challenge.

---

## Windows

On Windows, everything runs inside **WSL** (which gives you Linux inside
Windows). If you've never used WSL before, start at Step 1. If you already have
Ubuntu set up, you can skip to Step 2.

!!! note "Two terminals — don't mix them up"
    You'll use **two** different terminal windows in these steps:

    - **PowerShell** (built into Windows) — used **once** in Step 1 to install
      WSL.
    - **Ubuntu** (your WSL/Linux terminal) — used for **everything else**,
      including running the challenge.

    Both open from the **Start** menu: click Start and type the name
    (`PowerShell` or `Ubuntu`), then click the matching app.

### Step 1 — Install WSL

1. Open **Start**, type `PowerShell`, then **right-click** *Windows PowerShell*
   and choose **Run as administrator**. (Administrator is needed because this
   installs a Windows feature. Click **Yes** if Windows asks for permission.)
2. Type this command and press ++enter++:
   ```powershell
   wsl --install
   ```
   This installs WSL along with **Ubuntu** (the recommended Linux distribution).
   If WSL is already installed, this command will simply tell you so — no harm
   done.
3. **Restart your computer** if it asks you to.
4. After restarting open Ubuntu, and it will ask you to create a **username** and
   **password**. Pick something you'll remember — you'll need this password
   later whenever you run commands with `sudo`.

### Step 2 — Install Docker Desktop

1. Download and install **Docker Desktop** from
   <https://www.docker.com/products/docker-desktop/>.
2. Open Docker Desktop and turn on the WSL connection:
   - Go to **Settings > General** and tick *Use the WSL 2 based engine*.
   - Go to **Settings > Resources > WSL Integration** and turn on your
     distribution (Ubuntu).
3. Leave Docker Desktop running in the background while you do the challenge.

### Step 3 — Get the challenge files and run the setup script

1. Open your WSL terminal: click **Start**, type `Ubuntu`, and open the
   **Ubuntu** app. A black window opens with a prompt ending in `$`, waiting for
   you to type. (You can also use the **Windows Terminal** app if you have it —
   just make sure the tab says *Ubuntu*, not *PowerShell*.) New to the terminal?
   Read [WSL Basic Commands](hints.md#wsl-basic-commands) first — it covers
   moving between folders and running commands.
2. Get a copy of the challenge repository (see
   [Getting the challenge files](#getting-the-challenge-files) below).
3. Move into the folder you just downloaded and run the setup script.

    !!! warning "Make sure the files are inside WSL first"
        The setup only works if the files live **inside** WSL, not on the
        Windows side. If you downloaded the ZIP without changing the download location it went to
        your Windows *Downloads* folder by default, which is the wrong place —
        `chmod +x` won't stick and the challenge will run slowly. See
        [Getting the challenge files](#getting-the-challenge-files) for how to
        put them in the right place (your home folder inside Ubuntu).

    First move into the folder. If you unzipped the download, GitHub names the
    folder `UOPComputingChallenge-main` (with `-main` on the end); if you used
    `git clone` it's just `UOPComputingChallenge`. Use whichever matches:
   ```shell
   cd ~/UOPComputingChallenge-main   # if you downloaded the ZIP
   # or
   cd ~/UOPComputingChallenge        # if you cloned with git
   ```

    !!! tip "Not sure what's in your home folder?"
        Type `ls ~` and press ++enter++ to list the folders in your home
        directory, then `cd ~/` followed by the name you see. New to the
        terminal? [WSL Basic Commands](hints.md#wsl-basic-commands) covers
        moving between folders.

    Then run the setup script:
   ```shell
   chmod +x install-tools.sh
   sudo ./install-tools.sh
   ```
   `chmod +x` makes the script runnable (needed if you download the files as a **.zip**). 
   `sudo` runs the command with admin rights and will ask for the password you created in Step 1.

!!! tip
    You may need to close and reopen the terminal afterwards for everything to
    take effect.

---

## Linux

You'll need:

- A supported package manager: `apt`, `dnf`, or `yum` (most common Linux
  systems have one of these).
- Permission to run `sudo` (admin rights).

Steps:

1. Open a terminal.
2. Get a copy of the challenge repository (see
   [Getting the challenge files](#getting-the-challenge-files) below).
3. Move into the folder and run the setup script. If you unzipped the
   download, the folder is named `UOPComputingChallenge-main`; if you used
   `git clone` it's `UOPComputingChallenge`. Use whichever matches (run `ls`
   to check):
   ```shell
   cd UOPComputingChallenge-main   # if you downloaded the ZIP
   # or
   cd UOPComputingChallenge        # if you cloned with git

   chmod +x install-tools.sh
   sudo ./install-tools.sh
   ```
   `chmod +x` makes the script runnable (needed because downloading it as a ZIP
   removes that permission).

!!! tip
    You may need to open a new terminal afterwards for the changes to take
    effect. After Docker is installed, you may also need to log out and back in
    so you can use Docker without `sudo`.

---

## Mac

You'll need:

- **Homebrew** (the setup script installs it for you if it's missing).
- **Docker Desktop**, opened at least once so its engine is running. download from <https://docs.docker.com/desktop/setup/install/mac-install/>

Steps:

1. Open the **Terminal** app.
2. Get a copy of the challenge repository (see
   [Getting the challenge files](#getting-the-challenge-files) below).
3. Move into the folder and run the setup script **without** `sudo`. If you
   unzipped the download, the folder is named `UOPComputingChallenge-main`; if
   you used `git clone` it's `UOPComputingChallenge`. Use whichever matches
   (run `ls` to check):
   ```shell
   cd UOPComputingChallenge-main   # if you downloaded the ZIP
   # or
   cd UOPComputingChallenge        # if you cloned with git

   chmod +x install-tools.sh
   ./install-tools.sh
   ```
   `chmod +x` makes the script runnable (needed because downloading it as a ZIP
   removes that permission).

!!! warning "Don't use sudo on macOS"
    On a Mac, run the script **without** `sudo`. Homebrew refuses to run as an
    administrator, and the script will stop you if you try.

---

## Getting the challenge files

You need a local copy of the
[UOPComputingChallenge](https://github.com/WebOpsLR/UOPComputingChallenge)
repository. It contains the setup script and template files used in the
challenge.

There are two ways to get the files: **download the ZIP** (easiest if you're
not familiar with git) or **clone with git**. The exact steps differ per
operating system — follow the tab for **your** system.

=== "Windows"

    On Windows the files **must** end up inside your Ubuntu (WSL) home folder,
    not on the Windows side. Files kept under Windows (paths that start with
    `/mnt/c/...`, such as your normal *Downloads* folder) lose the "runnable"
    permission and make the challenge run slowly.

    **Option A — Download the ZIP (easiest)**

    Downloading the ZIP in your browser always saves it to the Windows side, so
    you'll need to move it into WSL afterwards.

    1. Open <https://github.com/WebOpsLR/UOPComputingChallenge> in your browser.
    2. Click the green **Code** button, then **Download ZIP**. It saves to your
       Windows *Downloads* folder.
    3. Open your *Downloads* folder in **File Explorer**, right-click the ZIP
       (`UOPComputingChallenge-main.zip`) and choose **Extract All...**. This
       creates a folder named `UOPComputingChallenge-main`, still on the
       Windows side.
    4. Move that folder into your Ubuntu home folder. Open your **Ubuntu**
       terminal and run (replace `<your-windows-username>` with your actual
       Windows account name):
       ```shell
       cp -r "/mnt/c/Users/<your-windows-username>/Downloads/UOPComputingChallenge-main" ~/
       ```
       Not sure of your Windows username? Run `ls /mnt/c/Users` to see the
       folder names.

    **Option B — Clone with git**

    If you're comfortable with **git**, clone straight into your WSL home folder
    from the **Ubuntu** terminal — this avoids the move entirely:
    ```shell
    cd ~
    git clone https://github.com/WebOpsLR/UOPComputingChallenge.git
    # or, if you have SSH set up with GitHub:
    git clone git@github.com:WebOpsLR/UOPComputingChallenge.git
    ```

    !!! success "How to know you're in the right place"
        Run `ls ~` — you should see the challenge folder in the list
        (`UOPComputingChallenge-main` if you downloaded the ZIP, or
        `UOPComputingChallenge` if you cloned with git). If it's there, `cd`
        into it and you're set. If `ls ~` **doesn't** show it (or `cd` reports
        `No such file or directory`), it's still on the Windows side and hasn't
        been moved into WSL — go back and move it.

=== "Linux"

    **Option A — Download the ZIP**

    1. Open <https://github.com/WebOpsLR/UOPComputingChallenge> in your browser.
    2. Click the green **Code** button, then **Download ZIP**.
    3. Extract it somewhere easy to find, such as your home folder. Your file
       manager can do this, or from a terminal:
       ```shell
       cd ~/Downloads
       unzip UOPComputingChallenge-main.zip -d ~/
       ```
       This creates `~/UOPComputingChallenge-main`.

    **Option B — Clone with git**

    ```shell
    cd ~
    git clone https://github.com/WebOpsLR/UOPComputingChallenge.git
    # or, if you have SSH set up with GitHub:
    git clone git@github.com:WebOpsLR/UOPComputingChallenge.git
    ```

    Run `ls ~` to confirm the folder is there
    (`UOPComputingChallenge-main` from the ZIP, or `UOPComputingChallenge`
    from git).

=== "Mac"

    **Option A — Download the ZIP**

    1. Open <https://github.com/WebOpsLR/UOPComputingChallenge> in your browser.
    2. Click the green **Code** button, then **Download ZIP**.
    3. Double-click the downloaded ZIP in **Finder** to unzip it — this creates
       a `UOPComputingChallenge-main` folder in your *Downloads*. Move it
       somewhere easy to find, such as your home folder, or from the **Terminal**
       app:
       ```shell
       mv ~/Downloads/UOPComputingChallenge-main ~/
       ```

    **Option B — Clone with git**

    ```shell
    cd ~
    git clone https://github.com/WebOpsLR/UOPComputingChallenge.git
    # or, if you have SSH set up with GitHub:
    git clone git@github.com:WebOpsLR/UOPComputingChallenge.git
    ```

    Run `ls ~` to confirm the folder is there
    (`UOPComputingChallenge-main` from the ZIP, or `UOPComputingChallenge`
    from git).

---

## What the setup script installs

The `install-tools.sh` script installs and checks everything you need, so you
don't have to install each tool by hand. It sets up and verifies:

- **Docker** — runs the app containers.
- **kubectl** — controls the Kubernetes cluster.
- **Minikube** — runs a small Kubernetes cluster on your machine.
- **Helm** — installs apps into Kubernetes.
- **Terraform** — sets everything up automatically from config files.

!!! info
    The steps above run `chmod +x install-tools.sh` before the script because
    downloading the files as a ZIP removes the "runnable" permission. If you
    cloned with git instead, this step is harmless — it just leaves the script
    runnable.

!!! warning "Run with sudo on Linux and WSL"
    On **Linux** and **Windows (WSL)** the script **must be run with `sudo`** —
    it installs system software that needs admin rights. On **macOS** do the
    opposite and run it **without** `sudo`.
