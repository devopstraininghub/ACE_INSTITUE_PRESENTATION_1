
## Module 1 — Linux Commands

### File commands

| Command | Description |
|---|---|
| `sudo su -` | Switching  root user |
| `touch filename` | Create the file |
| `mkdir dirname` | Create the directory |
| `ls` | Directory / files listing |
| `ll` | Long listing of files/directories |
| `ls -la` | Formatted listing with hidden files |
| `ls -lt` | Formatted listing sorted by time modified |
| `ls -lrth` | Formatted listing sorted by time modified, reverse order |
| `cd` | Change directory to home directory |
| `cd /` | Change directory to root |
| `cd /opt` | Navigate into the `/opt` directory |
| `cd dirname` | Change directory to `dirname` |
| `cd ..` | Go up one directory |
| `cd ../../` | Go up two directories |
| `cd -` | Go to the previous directory |
| `cd ~` | Change directory to the user's home directory |
| `pwd` | Show current working directory |
| `mkdir dir1 dir2 dir3` | Create multiple directories at once |
| `mkdir -p d1/d2/d3/d4` | Create nested directories |
| `cat filename` | View the contents of a file |
| `more filename` | Page through the contents of a file |
| `head filename` | Output the first 10 lines of a file |
| `tail filename` | Output the last 10 lines of a file |
| `tail -n filename` | Output only the last N lines |
| `touch file1 file2 file3` | Create multiple files at once |
| `rm file` | Delete a file |
| `rm -r dir` | Delete a directory |
| `rm -f file` | Force-remove a file |
| `rm -rf dir` | Force-remove a directory |
| `cp file1 file2` | Copy the contents of `file1` to `file2` |
| `cp -r dir1 dir2` | Copy `dir1` to `dir2`, creating `dir2` if it doesn't exist |
| `mv file1 file2` | Rename or move `file1` to `file2` |
| `du` | Show directory/file space usage |
| `du -sh` | Folder size in human-readable format |
| `du -h` | File size in human-readable format |
| `free` | Show memory and swap usage |

**`who -r`** — displays the current run level (`init 0`–`6`):

| Run level | Meaning |
|---|---|
| 0 | Halt / stop |
| 1 | Single-user mode |
| 2 | Multi-user, no network |
| 3 | Multi-user, with network |
| 4 | Unused |
| 5 | Multi-user, with GUI |
| 6 | Reboot |

### Network commands

| Command | Description |
|---|---|
| `ping google.com` | Check connectivity between two nodes |
| `whois domain` | Get WHOIS information for a domain |
| `dig domain` | Get DNS information for a domain |
| `wget url` | Download a file |
| `netstat` | Display connection information |

### Compression commands

| Command | Description |
|---|---|
| `zip -r data.zip filename` | Create `data.zip` containing `filename` |
| `unzip data.zip` | Extract files from a `.zip` archive |
| `tar -xvf file.tar` | Extract files from a `.tar` archive |
| `tar cvf file.tar.gz filename` | Archive `filename` into a `.tar.gz` |

---

## Module 2 — Why Docker? Problems before Docker 

### What is Docker?

Before Docker, developers built applications on their own laptops. When that same application moved to Testing or Production, it broke — not because the code was wrong, but because the environment was different: a different OS, a different software version, missing libraries, missing dependencies.

**Example — same app, two machines**

| | Developer Laptop | Production Server |
|---|---|---|
| Python | 3.12 | 3.8 |
| Flask | 3.0 | missing |
| OS | Ubuntu | CentOS |

Result: the application fails on deploy.

### What Docker packages

Docker bundles the application together with everything it needs to run into one unit called a container:

- Application code
- Runtime
- Libraries
- Dependencies
- Configuration

**Definition:** Docker is a containerization platform that packages an application along with all its dependencies so it can run anywhere without changing anything.

### Why Docker

| Before Docker | With Docker |
|---|---|
| "Works on my laptop" syndrome | Portable — same image runs everywhere |
| Version mismatches | Lightweight footprint |
| Library mismatches | Fast startup |
| Manual installation per server | Easy deployment |
| Slow deployment | Same environment, laptop to prod |

---

## Module 3 — Virtual Machine vs Docker 

A VM virtualizes the whole computer. Docker virtualizes just the application layer — that's the whole reason containers start in seconds, not minutes.

| Layer | Virtual Machine | Docker |
|---|---|---|
| 1 (top) | Application | Application |
| 2 | Guest OS *(full copy)* | Docker Engine |
| 3 | Hypervisor | — |
| 4 (bottom) | Host OS | Host OS |

- VM ships a full guest OS per instance → boots in **minutes**
- Docker shares the host kernel → starts in **seconds**

---

## Module 4 — Docker Architecture 

```
Docker Client  →  Docker Daemon  →  Docker Image  →  Docker Container  →  Docker Registry
```

| Component | Role |
|---|---|
| Docker Client | Commands typed by the user, e.g. `docker run nginx` |
| Docker Daemon | Runs in the background. Creates containers, downloads images. |
| Docker Image | A template. Cannot be executed directly. |
| Docker Container | A running instance of an image. |
| Docker Registry | Stores images — e.g. Docker Hub. |

<img width="1009" height="527" alt="image" src="https://github.com/user-attachments/assets/65ea46cd-230f-4c62-93e7-2eee9e3b7448" />

---

## Module 5 — Install Docker Desktop 

**Requirements**

## Supported Operating Systems

 Windows 11 (64-bit)

 Windows 10 (64-bit)

Recommended:

* Windows 10 Version 22H2
* Windows 11 Latest Version

## Hardware Requirements

Minimum:

* 64-bit Processor
* Intel VT-x or AMD-V support
* 4 GB RAM

Recommended:

* 8 GB RAM or more
* SSD
* Intel i5 / Ryzen 5 or above

Docker recommends at least **8 GB RAM** for smooth operation.

# Install WSL2

Open **PowerShell as Administrator**

Run

```powershell
wsl --install
```

This command:

* Installs WSL
* Downloads Linux Kernel
* Installs Ubuntu
* Sets WSL2 as default

Restart the PC.

---

## Verify Installation

Run

```powershell
wsl --status
```

```powershell
docker --version
docker info
```

---

## Module 6 — Docker Hands-on Commands 

### Version & info

```powershell
docker --version   # one line
docker version     # client + server details
docker info         # full engine state
```

### Search & pull images

```powershell
docker search nginx
docker pull nginx      # downloads the image — no container yet
docker images          # view what's on disk
```

### Run a container

```powershell
docker run nginx       # starts, then exits — nginx runs in the foreground
docker run -it ubuntu  # -i interactive, -t terminal
exit                   # stops the container
```

To leave a container running instead of exiting it, detach with `Ctrl+P` then `Ctrl+Q`.

### Container lifecycle

```powershell
docker ps                 # running containers
docker ps -a               # all containers, incl. stopped
docker start <name>
docker stop <name>
docker restart <name>
docker pause <name>
docker unpause <name>
docker rm <name>           # remove a stopped container
docker rm -f <name>        # force-remove, running or not
docker rmi <image>         # remove an image
```

| Command | Behavior |
|---|---|
| `docker run` | Creates a **new** container from an image every time. |
| `docker start` | Restarts an **existing** stopped container — no new container is created. |

### Exec vs attach

```powershell
docker exec -it <name> bash   # open a new shell inside the container
docker attach <name>          # attach to the container's existing process
```

| Command | Behavior |
|---|---|
| `docker exec` | Starts a **new** process/shell inside the container. Safe to exit without stopping it. |
| `docker attach` | Connects to the container's **main, already-running** process. Exiting it can stop the container. |

### Port mapping & naming

```powershell
docker run -d -p 8080:80 nginx     # host 8080 → container 80
docker run -d --name web nginx     # give the container a readable name
```

Open `http://localhost:8080` — the request routes straight into the container's port 80.

### Logs

```powershell
docker logs web
docker logs -f web    # -f follows the log stream live
```

### Cleanup

```bash
# Git Bash / WSL / Linux
docker rm -f $(docker ps -aq)     # remove all containers
docker rmi $(docker images -q)    # remove all images
```

```powershell
# PowerShell equivalent
docker ps -aq | ForEach-Object { docker rm -f $_ }
docker images -q | ForEach-Object { docker rmi -f $_ }
```

Command substitution (`$(...)`) is a Bash construct — PowerShell needs the `ForEach-Object` pipeline instead.

---

## Module 7 — Dockerfile: Building Your Own Image 

Everything so far pulled a pre-built image (`nginx`, `ubuntu`) from Docker Hub. Real projects need a custom image — the app's own files baked in on top of a base image.

```
Dockerfile  →  docker build  →  Docker Image  →  docker run  →  Docker Container
```

### Common instructions

| Instruction | Purpose |
|---|---|
| `FROM` | Base image everything else builds on top of |
| `WORKDIR` | Working directory inside the image for the instructions that follow |
| `COPY` | Copies files from the build context into the image |
| `RUN` | Executes a command at build time — typically installing dependencies |
| `EXPOSE` | Documents which port the container listens on |
| `CMD` | Default command that runs when the container starts |

### Worked example — Dockerizing a Python Flask web app

**The app** — `/opt/app.py`

```python
import os
from flask import Flask

app = Flask(__name__)

@app.route("/")
def main():
    return "Welcome to Batch17d!"

@app.route("/how are you")
def hello():
    return "I am good, how about you?"

if __name__ == "__main__":
    app.run()
```

**How you'd run this by hand on a server** — this is the manual setup a Dockerfile replaces:

```bash
yum update -y
yum install python3 python3-pip pip -y
pip install flask
FLASK_APP=/opt/python_app/app.py flask run --host=0.0.0.0 --port=8080
```

Every one of those steps has to be repeated, in order, on every server that runs this app. A Dockerfile bakes them into an image once.

**The Dockerfile**

```dockerfile
FROM amazonlinux
RUN yum update -y
RUN yum install python3 python3-pip pip -y
RUN pip install flask
COPY app.py /opt/python_app/app.py
CMD FLASK_APP=/opt/python_app/app.py flask run --host=0.0.0.0 --port=8080
EXPOSE 8080
```

Line by line:

1. `FROM amazonlinux` — start from a bare Amazon Linux base image.
2. `RUN yum update -y` — update the package index, as its own layer.
3. `RUN yum install python3 python3-pip pip -y` — install Python and pip.
4. `RUN pip install flask` — install the Flask framework.
5. `COPY app.py /opt/python_app/app.py` — copy the app source into the image.
6. `CMD FLASK_APP=... flask run --host=0.0.0.0 --port=8080` — the command that starts the app when the container runs. `--host=0.0.0.0` is required — without it, Flask only listens inside the container and Docker's port mapping has nothing to forward to.
7. `EXPOSE 8080` — documents that the container listens on 8080.

**Build and run**

```bash
docker build -t pythonimg:v1 -f Dockerfile .
docker run -d -p 8080:8080 pythonimg:v1
```

Test both routes in a browser:

- `http://localhost:8080/` → "Welcome to Ace !"
- `http://localhost:8080/how%20are%20you` → "I am good, how about you?" (the space in the route needs to be URL-encoded as `%20`)

| Flag | Meaning |
|---|---|
| `-t pythonimg:v1` | Tags (names) the built image |
| `-f Dockerfile` | Explicitly points at the Dockerfile to use |
| `.` | Build context — the folder containing `app.py` and the Dockerfile |

---
---

## Lab

Solo, in order — each step depends on the one before it.

1. Install Docker Desktop.
2. Verify the installation (`docker --version`, `docker info`).
3. Pull the `ubuntu` image.
4. Create an Ubuntu container.
5. Run `pwd`, `ls`, `mkdir`, `touch` inside the container.
6. Exit the container, then restart it.
7. Run an Nginx container with port mapping (`8080:80`).
8. Open `http://localhost:8080` and confirm it loads.
9. Build the Flask app image (`docker build -t pythonimg:v1 -f Dockerfile .`), run it on port 8080, and confirm both `/` and `/how%20are%20you` respond.
10. Remove all containers and images created during the lab.

---
