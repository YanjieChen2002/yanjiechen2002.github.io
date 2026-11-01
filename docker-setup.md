# Docker setup for this workshop

This workshop runs inside a Docker image. Your own Mac, Windows, or Linux machine only needs a working Docker installation. You do not install R, Python, or the clonality tools yourself.

Finish the checks in [Confirm that Docker works](#confirm-that-docker-works) before `docker compose build`. The build uses the `Dockerfile` and `docker-compose.yml` in this repository. Those files are not involved until the `docker` command itself runs.

## Install Docker and leave it running

Install **Docker Desktop** on macOS or Windows. On Linux, install **Docker Engine** and the **Compose v2 plugin**. The workshop command is `docker compose` (a space, Compose v2), which current Docker Desktop already includes.

Download pages:

- macOS and Windows: [Install Docker Desktop](https://docs.docker.com/desktop/)
- Linux: [Install Docker Engine](https://docs.docker.com/engine/install/) and the [Compose plugin](https://docs.docker.com/compose/install/linux/)

Docker Desktop must stay open while you build or run the workshop. Closing the window is fine. Quitting Docker Desktop stops the engine, and later commands cannot connect to it.

### macOS

1. Open **Apple menu → About This Mac** and note the chip: **Apple** (M1, M2, M3, …) or **Intel**.
2. Download the Docker Desktop disk image that matches that chip from the Mac install page.
3. Open the `.dmg`. Drag **Docker** onto the **Applications** folder. Eject the disk image when Finder finishes copying.
4. Start Docker Desktop with one of these methods:
   - In Finder, open **Applications** and double-click **Docker**.
   - Spotlight: press Command-Space, type `Docker`, and press Return.
   - In Terminal, run:

     ```bash
     open -a Docker
     ```

   Do not type `/Applications/Docker.app` as a command. On macOS an `.app` is a folder bundle, not a program you can execute that way. The shell reports `permission denied` even when Docker is installed correctly. Details are in [permission denied: /Applications/Docker.app](#permission-denied-applicationsdockerapp).
5. Accept the agreement. When macOS asks for your password, enter it. That step installs the background helper and the `docker` command-line tools. Cancelling it leaves Docker Desktop open but `docker` missing from the terminal.
6. Wait until the whale icon in the menu bar stops animating and the status says Docker Desktop is running. The first launch can take a few minutes.

### Windows

1. Install Docker Desktop from the Windows install page. When the installer offers it, enable the **WSL 2** backend.
2. Restart the computer if the installer asks you to.
3. Start **Docker Desktop** from the Start menu. Wait until the whale icon says Docker Desktop is running.
4. Use a new **PowerShell** or **Terminal** window for the commands below.

### Linux

Follow the Docker Engine instructions for your distribution, then install the Compose plugin. After installation, add your user to the `docker` group so you can run Docker without `sudo`:

```bash
sudo usermod -aG docker "$USER"
```

Log out and back in (or reboot) so the new group membership applies. Then continue with the checks below.

## Put the repository in a normal folder

Clone into your home directory, `Documents`, or another ordinary project folder:

```bash
cd ~
git clone https://github.com/dinhngockhanh/IICDWorkshopInferringClonality.git
cd IICDWorkshopInferringClonality
```

Avoid cloning into `~/Applications` or `/Applications`. Those folders are for applications. A checkout there still works, but it is easy to confuse with `/Applications/Docker.app`, which is the Docker program and is unrelated to this repository.

If the repository is already inside `Applications`, move it, then enter the new location:

```bash
mv ~/Applications/IICDWorkshopInferringClonality ~/Documents/
cd ~/Documents/IICDWorkshopInferringClonality
```

## Confirm that Docker works

Open a **new** terminal after Docker finishes starting. An already-open terminal often keeps the old `PATH` and still cannot find `docker`.

Run:

```bash
docker version
docker compose version
```

Both commands should print client and server (or Compose plugin) version numbers. Then, from the repository directory:

```bash
docker compose build
```

The first build downloads base images and compiles the workshop tools. It takes a long time and needs a network connection. Later builds are faster.

When the build finishes, list the workshop commands with:

```bash
docker compose run --rm workshop
```

## If something fails

Match the message you see. The first two are the ones produced by typing `docker compose build` before Docker Desktop has installed its command-line tools, then trying to launch the app bundle directly.

### `command not found: docker`

The shell never found a `docker` program. This Mac has not reached the workshop files yet. Typical causes:

- Docker Desktop is not installed.
- Docker Desktop is installed, but it has never completed its first launch, so the command-line tools were not installed.
- The password prompt was cancelled.
- The terminal window was opened before that install finished. Open a new window and try again.

Check the three places Docker Desktop puts the program:

```bash
ls -l /usr/local/bin/docker
ls -l "$HOME/.docker/bin/docker"
ls -l "/Applications/Docker.app/Contents/Resources/bin/docker"
```

How to read that:

| What you see | What to do |
| --- | --- |
| `No such file or directory` for the Docker.app path as well | Docker Desktop is not installed. Install it from the Mac page above, open it, and wait until it is running. |
| The file inside `Docker.app` exists, and `/usr/local/bin/docker` or `$HOME/.docker/bin/docker` also exists | Open a new terminal. Run `echo "$PATH"` and confirm that it contains the directory where `docker` was found (`/usr/local/bin` or `$HOME/.docker/bin`). |
| The file inside `Docker.app` exists, but the other two are missing | Docker Desktop has not installed the command-line tools. Open Docker Desktop, finish the password prompt, then use **Settings → Advanced** and install the CLI tools (System installs into `/usr/local/bin`; User installs into `$HOME/.docker/bin`). Open a new terminal afterward. |

A user-level path works without an administrator password when the copy inside the app is readable. Add this to `~/.zshrc`, then open a new terminal:

```bash
export PATH="$HOME/.docker/bin:/Applications/Docker.app/Contents/Resources/bin:$PATH"
```

`docker version` still needs Docker Desktop to be running. The client and the engine are separate: a `docker` binary on `PATH` does not start the engine by itself.

### `permission denied: /Applications/Docker.app`

`/Applications/Docker.app` exists. The shell refused to execute it because an `.app` bundle is a directory. macOS reports that attempt as `permission denied`. This message does not mean the workshop image is blocked, and it does not by itself mean the file permissions on Docker are wrong.

Start the app with:

```bash
open -a Docker
```

Wait until the menu-bar whale says Docker Desktop is running, open a new terminal, and run `docker version`.

If `open -a Docker` itself fails, or the same `permission denied` appears for the binary inside the bundle:

```bash
ls -ld /Applications/Docker.app
ls -l "/Applications/Docker.app/Contents/Resources/bin/docker"
```

a managed or lab Mac may be blocking that application. The workshop configuration cannot change those permissions. An administrator for that computer has to allow Docker Desktop to run and, if required, approve its privileged helper.

### `Cannot connect to the Docker daemon` or `docker.sock: connect: no such file or directory`

The `docker` command is installed, and Docker Desktop is not running or has not finished starting. Start it with `open -a Docker` (macOS) or from the Start menu (Windows), wait until it reports that it is running, and run `docker version` again.

### `permission denied` while connecting to `docker.sock`

On Linux, the user is usually not in the `docker` group yet. Run the `usermod` command in the Linux section, then log out and back in. On macOS or Windows this usually means Docker Desktop is still starting, or it was installed for another user account.

### Password, virtualization, or company-managed computers

Host names such as a shared lab workstation often mean the machine is managed centrally. Docker Desktop’s first launch needs permission to install a helper and to use virtualization. If the password dialog never appears, the dialog is rejected, or virtualization is turned off by policy, a local account cannot finish the setup. Send the diagnostic output below to the instructor or the machine’s administrator.

Colima, Podman, or a remote Linux server are possible substitutes only when that machine already provides them and `docker compose build` works there. They are outside this repository’s tested setup.

## What to send if you are still stuck

From a new terminal, after trying `open -a Docker`, paste the output of:

```bash
sw_vers
uname -m
echo "$PATH"
ls -ld /Applications/Docker.app
ls -l /usr/local/bin/docker "$HOME/.docker/bin/docker" "/Applications/Docker.app/Contents/Resources/bin/docker"
docker version
docker compose version
```

On Windows, send the output of `docker version` and a screenshot of the Docker Desktop status instead of the `sw_vers` lines. On Linux, send `docker version`, `docker compose version`, and `groups`.

Include the exact command you typed and the full error text. With that output it is clear whether Docker is missing, installed but not running, or blocked by the computer’s own permissions.
