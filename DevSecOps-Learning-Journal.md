# DevSecOps Learning Journal

---

| Field              | Details                          |
|--------------------|----------------------------------|
| **Author**         | *Adani Kamal*                    |
| **Date**   | June 2026                    |
| **Program**        | DevSecOps Foundations   |
| **Objective**  | Build, secure, and automate a complete DevSecOps pipeline from scratch |

---

> **About this document**
> This journal serves two purposes: a personal learning reference that explains every concept in depth, and a professional progress report. Each section explains *what* was done, *why* it matters in the real world, *how* it works internally, and what best practices apply.

# Table of contents

- [DevSecOps Learning Journal](#devsecops-learning-journal)
- [Table of contents](#table-of-contents)
- [Step 1 – Docker Desktop and Docker Compose](#step-1--docker-desktop-and-docker-compose)
  - [Objective](#objective)
  - [Docker](#docker)
  - [Core Concepts](#core-concepts)
  - [Docker Compose File Explained](#docker-compose-file-explained)
  - [Architecture Diagram](#architecture-diagram)
  - [Commands](#commands)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 2 – Deploying DVWA (Vulnearble Machine)](#step-2--deploying-dvwa-vulnearble-machine)
  - [DVWA](#dvwa)
  - [Database Initialisation](#database-initialisation)
  - [Vulnerability Categories](#vulnerability-categories)
  - [Difficulty Levels](#difficulty-levels)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 3 – GitHub](#step-3--github)
  - [Objective](#objective)
  - [Creating a New Repository](#creating-a-new-repository)
  - [Command](#command)
  - [Problems Encountered](#problems-encountered)
  - [Successful Push](#successful-push)
  - [Multiple empty folder created](#multiple-empty-folder-created)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 4 – GitHub Actions Workflow](#step-4--github-actions-workflow)
  - [Objective](#objective)
  - [Why GitHub Actions?](#why-github-actions)
  - [Workflow Architecture](#workflow-architecture)
  - [Workflow File](#workflow-file)
  - [Understanding the Workflow](#understanding-the-workflow)
  - [Execution Result](#execution-result)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 5 – Super-Linter (CI Quality Gate)](#step-5--super-linter-ci-quality-gate)
  - [Objective](#objective)
  - [Super-Linter](#super-linter)
  - [Workflow Configuration](#workflow-configuration)
  - [Why Only YAML Was Validated](#why-only-yaml-was-validated)
  - [Problems Encountered](#problems-encountered)
  - [Successful Execution](#successful-execution)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 6 – Gitleaks (Secret Scanning)](#step-6--gitleaks-secret-scanning)
  - [Objective](#objective)
  - [Gitleaks](#gitleaks)
  - [The important](#the-important)
  - [Workflow Configuration](#workflow-configuration)
  - [Understanding the Workflow](#understanding-the-workflow)
  - [Successful Execution](#successful-execution)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 7 – Semgrep (Static Application Security Testing)](#step-7--semgrep-static-application-security-testing)
  - [Objective](#objective)
  - [What is SAST?](#what-is-sast)
  - [Semgrep](#semgrep)
  - [Position in the DSO Pipeline](#position-in-the-dso-pipeline)
  - [Workflow Configuration](#workflow-configuration)
  - [Understanding the Workflow](#understanding-the-workflow)
  - [Initial Findings](#initial-findings)
  - [Why This Was Reported](#why-this-was-reported)
  - [Trade-offs](#trade-offs)
  - [Successful Execution](#successful-execution)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 8 – Trivy Filesystem Scan (Software Composition Analysis)](#step-8--trivy-filesystem-scan-software-composition-analysis)
  - [Objective](#objective)
  - [Software Composition Analysis (SCA)](#software-composition-analysis-sca)
  - [What is Trivy?](#what-is-trivy)
  - [Position in the DevSecOps Pipeline](#position-in-the-devsecops-pipeline)
  - [Workflow Configuration](#workflow-configuration)
  - [Understanding the Workflow](#understanding-the-workflow)
  - [Workflow Hardening](#workflow-hardening)
  - [Successful Execution](#successful-execution)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 9 – Containerization with Docker Compose](#step-9--containerization-with-docker-compose)
  - [Objective](#objective)
  - [Why Docker?](#why-docker)
  - [Why Docker Compose?](#why-docker-compose)
  - [Docker Compose Configuration](#docker-compose-configuration)
  - [Understanding the Configuration](#understanding-the-configuration)
  - [Deployment](#deployment)
  - [Why DVWA?](#why-dvwa)
  - [Role in the DevSecOps Pipeline](#role-in-the-devsecops-pipeline)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 10 – Trivy Container Image Scan](#step-10--trivy-container-image-scan)
  - [Objective](#objective)
  - [Why Scan Docker Images?](#why-scan-docker-images)
  - [Filesystem Scan vs Container Image Scan](#filesystem-scan-vs-container-image-scan)
  - [Workflow Configuration](#workflow-configuration)
  - [Understanding the Workflow](#understanding-the-workflow)
  - [Workflow Hardening](#workflow-hardening)
  - [Successful Execution](#successful-execution)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Step 11 – OWASP ZAP (Dynamic Application Security Testing)](#step-11--owasp-zap-dynamic-application-security-testing)
  - [Objective](#objective)
  - [Why DAST?](#why-dast)
  - [Implementation](#implementation)
  - [Issues Encountered](#issues-encountered)
  - [Scan Summary](#scan-summary)
  - [Evidence](#evidence)
  - [Reflection](#reflection)
- [Milestone Achieved](#milestone-achieved)
  - [DevSecOps Pipeline](#devsecops-pipeline)
- [REFERENCE](#reference)

---
# Step 1 – Docker Desktop and Docker Compose

## Objective

Install Docker Desktop, understand the Docker ecosystem, create a `docker-compose.yml` file, and deploy the first containerised application in a local lab environment.

## Docker

Docker is one of the most important tools in a DevSecOps engineer's toolkit.

1. **Consistency across environments.** One of the oldest problems in software is "it works on my machine." Docker solves this by packaging an application together with everything it needs. Its OS libraries, runtime, configuration, and code into a single portable unit called a *container*. This container behaves identically whether it runs on a developer's laptop, a CI/CD pipeline server, or a production Kubernetes cluster.

2. **Security isolation.** Each container runs in its own isolated process namespace. If a running container is compromised, the blast radius is limited to that container rather than the entire host machine. DevSecOps engineers use this isolation to limit lateral movement by attackers.

3. **Reproducible security testing.** Security tools like vulnerability scanners and dynamic application security testing (DAST) tools need a consistent target to scan. Docker makes it trivial to spin up a known-good (or intentionally vulnerable) application, scan it, tear it down, and repeat. This is exactly what we are doing with DVWA in this lab.

4. **Pipeline integration.** Every major CI/CD platform (GitHub Actions, GitLab CI, Jenkins) can run steps inside Docker containers. This means the build, test, lint, and security-scan steps all run in a clean, reproducible environment every single time.

## Core Concepts

Understanding the difference between a Docker Image, a Docker Container, and Docker Compose is fundamental before executing any commands.

### Docker Image

A Docker Image is a **read-only template** used to create containers. It contains:

- A base operating system layer (e.g., Debian, Alpine Linux)
- Application code
- Runtime dependencies (e.g., Apache, PHP, Python)
- Configuration files

Images are built from a file called a `Dockerfile` or pulled from a public registry like [Docker Hub](https://hub.docker.com). They are **immutable**. Once built, an image does not change. This immutability is a security property. We can cryptographically verify that the image that are running is the same image that was scanned and approved.

### Docker Container

A Docker Container is a **running instance** of an image. When Docker starts an image, it creates a writable layer on top of the read-only image layers and starts the processes defined in the image. Multiple containers can run from the same image simultaneously.

| Concept   | Analogy                                |
|-----------|----------------------------------------|
| Image     | A cookie cutter (template)             |
| Container | A cookie (a live instance from the template) |

Containers are **ephemeral** by design. When stopped and removed, all data written inside them is gone unless a persistent volume is explicitly mounted. This ephemerality is both a feature and a pitfall for security. It means compromised containers can be destroyed and replaced, but it also means logs and forensic data can be lost.

### Docker Compose

Docker Compose is a tool for **defining and running multi-container applications** using a single YAML configuration file (`docker-compose.yml`). Instead of running long `docker run` commands with many flags, I describe the entire application stack in YAML and start everything with one command.

For DevSecOps engineers, Compose is invaluable because:

- It documents the exact configuration of the lab environment in version-controlled code.
- It makes environments reproducible. Any team member can clone the repository and reproduce the identical environment.
- Multi-service stacks (e.g., application + database + security scanner) are declared once and started atomically.

### Environment

| Component        | Status      | Notes                                       |
|------------------|-------------|---------------------------------------------|
| Docker Desktop   | ✅ Installed | Installed via `.dmg` installer on macOS     |
| Docker Engine    | ✅ Running   | Verified via `docker ps`                    |
| Docker Compose   | ✅ Available | Bundled with Docker Desktop (v2+)           |
| Host Platform    | `linux/arm64/v8` | Apple Silicon Mac (M-series chip)    |

### Installation Steps Performed

1. Downloaded `Docker.dmg` from the official Docker website.
2. Double-clicked `Docker.dmg` and dragged the Docker icon into the `/Applications` folder.
3. Launched Docker Desktop from `/Applications`.
4. Accepted the Docker Subscription Service Agreement.
5. Verified Docker was running by observing the Docker whale icon in the macOS menu bar.

> **Why this matters:** Docker Desktop is the local runtime environment for the entire lab. Without it, no containers can be built or run on the developer workstation. Verifying the installation before proceeding is a standard pre-flight check.

## Docker Compose File Explained

The following file was created at `devsecops-lab/docker-compose.yml`:

```yaml
services:
  dvwa:
    image: vulnerables/web-dvwa
    container_name: dvwa
    ports:
      - "8080:80"
    restart: unless-stopped
```

### Line-by-Line Explanation

| Key               | Value                    | What it means |
|-------------------|--------------------------|---------------|
| `services`        | *(top-level key)*        | Declares the list of containers that Compose will manage. Each named entry under `services` is one container. |
| `dvwa`            | *(service name)*         | The logical name of this service within the Compose network. Other services could reference it by this name. |
| `image`           | `vulnerables/web-dvwa`   | Tells Docker to pull this image from Docker Hub if it is not already available locally. `vulnerables` is the Docker Hub namespace and `web-dvwa` is the image name. |
| `container_name`  | `dvwa`                   | Overrides the auto-generated container name. Makes the container easy to reference by name in commands like `docker logs dvwa`. |
| `ports`           | `"8080:80"`              | - Maps port `8080` on the **host machine** (my Mac) to port `80` inside the **container**. <br> - The format is `HOST_PORT:CONTAINER_PORT` </br> - **Inside the container**, Apache is listening on port `80` (standard HTTP). <br> - **On Mac**, port `80` is often restricted to root-level processes. Port `8080` is an unprivileged alternative commonly used in development. <br> - Docker acts as a network bridge when the browser requests `http://localhost:8080`, the Docker engine intercepts the traffic and forwards it to port `80` inside the `dvwa` container. </br> |
| `restart`         | `unless-stopped`         | Instructs Docker to automatically restart the container if it crashes or if Docker Desktop is restarted — unless the container was explicitly stopped by the user. |

```
Browser → localhost:8080 → Docker NAT → Container Port 80 → Apache → DVWA
```

> **Best practice:** Never expose containers directly on port `80` or `443` in a local lab. Using a non-standard host port like `8080` keeps things explicit and avoids conflicts with other local services.

## Architecture Diagram

The following diagram illustrates the network path from the browser to the DVWA application running inside Docker.

![Docker Desktop Installation](Evidence/DVWA-Local.png)

## Commands

### Command 1

```bash
docker compose up -d
```

| Part             | Meaning |
|------------------|---------|
| `docker`         | The Docker CLI |
| `compose`        | Invokes the Docker Compose plugin (v2 syntax, replaces the older `docker-compose` binary) |
| `up`             | Creates and starts all services defined in `docker-compose.yml` |
| `-d`             | **Detached mode** — runs containers in the background, freeing up the terminal |

**What happens internally:**

1. Docker reads `docker-compose.yml` from the current directory.
2. For each service, it checks if the specified image (`vulnerables/web-dvwa`) exists in the local image cache.
3. If the image is not cached, Docker pulls it layer by layer from Docker Hub.
4. Docker creates a virtual network for the Compose project (named `devsecops-lab_default` by convention).
5. Docker creates the container using the image, applies the port mapping and restart policy.
6. Docker starts the container processes (Apache, MySQL, PHP-FPM).
7. Control returns to the terminal immediately because of `-d`.

**Expected output:**
```
[+] Running 2/2
 ✔ Network devsecops-lab_default  Created
 ✔ Container dvwa                 Started
```

> **Why it is useful:** The `-d` flag is essential in CI/CD pipelines and production deployments. A long-running service should never hold open a terminal session or block the next step in a pipeline.
<br> **Common mistake:** Omitting `-d` during testing is fine for watching logs in real time, but forgetting it in a pipeline will cause the pipeline to hang indefinitely waiting for the container to stop.

### Command 2

```bash
docker ps
```

| Part      | Meaning |
|-----------|---------|
| `docker`  | The Docker CLI |
| `ps`      | Lists **running** containers (analogous to the Unix `ps` command for processes) |

**What happens internally:**

Docker queries the Docker Engine daemon for all currently running containers and returns a formatted table.

**Expected output:**
```
CONTAINER ID   IMAGE                    COMMAND                  CREATED         STATUS         PORTS                  NAMES
a1b2c3d4e5f6   vulnerables/web-dvwa     "/main.sh"               2 minutes ago   Up 2 minutes   0.0.0.0:8080->80/tcp   dvwa
```

**Understanding the output columns:**

| Column         | Meaning |
|----------------|---------|
| `CONTAINER ID` | Shortened unique identifier for the container |
| `IMAGE`        | The image the container was created from |
| `COMMAND`      | The entrypoint command running inside the container |
| `CREATED`      | How long ago the container was created |
| `STATUS`       | Current state: `Up X minutes` confirms it is healthy and running |
| `PORTS`        | Shows the port mapping: `0.0.0.0:8080->80/tcp` means all host interfaces on port 8080 forward to container port 80 |
| `NAMES`        | The human-readable container name (`dvwa`) |

**Why it is useful:** `docker ps` is the first diagnostic command to run whenever something is not working. If the container is not listed, it either failed to start or crashed. Use `docker ps -a` to see stopped containers and `docker logs dvwa` to read the error output.

### Command 3

```bash
docker compose down
```

| Part       | Meaning |
|------------|---------|
| `docker`   | The Docker CLI |
| `compose`  | Docker Compose plugin |
| `down`     | Stops and **removes** all containers, networks, and default volumes created by `up` |

**What happens internally:**

1. Docker sends a `SIGTERM` signal to the container's main process, giving it time to shut down gracefully.
2. After a timeout (default 10 seconds), if the process has not exited, Docker sends `SIGKILL`.
3. The container is stopped and removed.
4. The Compose-managed network is removed.
5. Named volumes are **not** removed unless `--volumes` is also passed.

**Expected output:**
```
[+] Running 2/2
 ✔ Container dvwa                 Removed
 ✔ Network devsecops-lab_default  Removed
```

**Why it is useful:** Always tear down containers when they are not in use. Leaving containers running consumes CPU and memory, and an exposed vulnerable application (like DVWA) is a security risk even on a local network.

**Important distinction — `down` vs `stop`:**

| Command              | What it does |
|----------------------|-------------|
| `docker compose stop` | Stops containers but does **not** remove them. They can be restarted with `docker compose start`. |
| `docker compose down` | Stops **and removes** containers and networks. A fresh `up` creates new containers. |

> **Best practice:** Use `down` in lab exercises to ensure a clean state for each session.

## Evidence

### Screenshot 1 – Docker Desktop Installation

![Docker Desktop Installation](Evidence/step1-Install-docker.png)

## Reflection

| Concept | Key Takeaway |
|---------|-------------|
| Docker Images | Immutable, layered blueprints for containers. Pulled from registries. |
| Docker Containers | Live, running instances of images. Ephemeral by default. |
| Docker Compose | Declarative YAML-based tool for managing multi-container applications. |
| Port Mapping | `HOST:CONTAINER` syntax bridges the host network to the container network. |
| Detached Mode (`-d`) | Essential for running containers in the background, especially in pipelines. |
| `docker ps` | Always run this to verify container state. |
| `docker compose down` | Clean teardown. Always run this after a lab session. |
| ARM Warning | Expected on Apple Silicon. Handled automatically by Docker Desktop via QEMU. Production deployments require architecture-matched images. |
| Image Security | In production, always verify image digests and scan images before deployment. |

---
# Step 2 – Deploying DVWA (Vulnearble Machine)

## DVWA

**DVWA** stands for **Damn Vulnerable Web Application**. It is an open-source PHP/MySQL web application that was **intentionally built to be insecure**. It was created by the team at [digininja](https://github.com/digininja/DVWA) specifically for security professionals, developers, and students to practice exploiting and defending against the most common web application vulnerabilities.

DVWA is a standard training tool used in:
- Penetration testing training
- Application security courses (OWASP, CEH, OSCP)
- Security awareness programmes
- DevSecOps pipeline validation (as a known-vulnerable DAST target)

### Why DVWA Exists

Teaching security without a safe, legal target is difficult. DVWA provides a realistic web application where I can:

- Practice SQL Injection without attacking a real website (which is illegal).
- Learn how XSS works by triggering it in a controlled environment.
- Understand CSRF, file upload vulnerabilities, command injection, and more.
- Validate that security scanning tools are working correctly — if a scanner cannot find DVWA's obvious vulnerabilities, the scanner is misconfigured.

### ⚠️ Critical Warning: DVWA Must Never Be Exposed to the Internet

DVWA is loaded with real, exploitable vulnerabilities by design. If it is ever accessible from the public internet:

- Attackers can trivially compromise the container.
- Depending on the Docker and network configuration, they may be able to pivot to the host machine or internal network.
- An attacker could inadvertently be handed a foothold on the network.

**Rule:** DVWA should only ever be accessible on `localhost` or a fully isolated private network. Binding to `127.0.0.1:8080` (loopback only) is safer than `0.0.0.0:8080` (all interfaces). Always run `docker compose down` when the lab session is complete.

### Deployment

DVWA was deployed using the `docker-compose.yml` file described in Step 1.

| Item              | Value                          |
|-------------------|--------------------------------|
| Image             | `vulnerables/web-dvwa`         |
| Container Name    | `dvwa`                         |
| Host URL          | `http://localhost:8080`        |
| Internal Port     | `80` (Apache)                  |
| Database Init URL | `http://localhost:8080/setup.php` |
| Status            | ✅ Successfully deployed        |

## Database Initialisation

DVWA requires its MySQL database to be populated before first use. This is done by:

1. Navigating to `http://localhost:8080/setup.php` in a browser.
2. Clicking **Create / Reset Database**.
3. Waiting for the confirmation message: *"Setup successful"*.
4. Being redirected to the login page at `http://localhost:8080/login.php`.

> **Why this step exists:** The `setup.php` script runs SQL `CREATE TABLE` and `INSERT` statements to build DVWA's schema. This is a common pattern in web applications. A setup or migration script that initialises the database on first run. In production applications, this is typically automated as part of the deployment pipeline (e.g., using database migrations in frameworks like Django, Rails, or Liquibase).

### Authentication

| Field     | Value      |
|-----------|------------|
| Username  | `admin`    |
| Password  | `password` |

#### Default Credentials Are Intentionally Insecure

DVWA ships with weak, well-known credentials by design. This serves several educational purposes:

1. **Demonstrates the risk of default credentials.** One of the most common real-world attack vectors is simply trying default usernames and passwords. Many IoT devices, network equipment, databases, and CMS platforms (WordPress, phpMyAdmin) are compromised this way.

2. **Allows focus on the application vulnerabilities** rather than credential management in a lab context.

3. **Represents a vulnerability category in itself** — Broken Access Control and Security Misconfiguration are both in the [OWASP Top 10](https://owasp.org/www-project-top-ten/).

> **Real-world practice:** In any real application, default credentials must be changed immediately after deployment. Automated credential rotation, secrets management (e.g., HashiCorp Vault, AWS Secrets Manager), and multi-factor authentication are standard controls.

## Vulnerability Categories

DVWA exposes the following vulnerability categories. Each one maps to real-world attack techniques used by malicious actors. Understanding these vulnerabilities is the foundation of application security.

| Vulnerability | Purpose in DVWA | Real-World Risk |
|---------------|-----------------|-----------------|
| **SQL Injection (SQLi)** | Practice crafting malicious SQL queries via input fields | Attacker can dump entire databases, bypass authentication, or delete data. One of the most critical web vulnerabilities. |
| **Command Injection** | Inject OS commands through application input | Attacker gains remote code execution (RCE) on the server. Equivalent to having shell access. |
| **Reflected XSS** | Inject script that is reflected back in the HTTP response | Attacker tricks a victim into clicking a crafted URL; the script runs in the victim's browser and can steal session cookies. |
| **Stored XSS** | Inject script that is permanently stored in the database | Script executes for every user who views the affected page. Used for credential harvesting, session hijacking, and defacement. |
| **DOM XSS** | Exploit client-side JavaScript that writes user input into the DOM unsafely | Similar impact to reflected XSS but harder to detect because the payload never touches the server — it lives entirely in the browser. |
| **CSRF (Cross-Site Request Forgery)** | Trick a logged-in user's browser into making unintended requests | Attacker can force a victim to change their password, transfer funds, or perform any action the victim is authorised to do. |
| **File Upload** | Upload malicious files (e.g., PHP webshells) disguised as images | Attacker achieves RCE by uploading a file that the server executes. Critical if upload validation is missing or bypassable. |
| **File Inclusion (LFI/RFI)** | Include local or remote files via unsanitised path parameters | Local File Inclusion (LFI) can expose `/etc/passwd`, SSH keys, and config files. Remote File Inclusion (RFI) can execute attacker-controlled code. |
| **Brute Force** | Practice automated login attempts against the DVWA login form | Without rate limiting, account lockout, or CAPTCHA, attackers can guess passwords using tools like Hydra or Burp Intruder. |
| **Weak Session IDs** | Analyse predictable session token generation | Predictable session tokens allow session hijacking — an attacker can guess or calculate a valid session token and impersonate a user. |
| **Insecure CAPTCHA** | Bypass poorly implemented bot-detection mechanisms | CAPTCHA implementations that can be bypassed via API manipulation or logic flaws provide no real protection against automation. |

## Difficulty Levels

DVWA allows switching between four difficulty levels for each vulnerability:

| Level    | Description |
|----------|-------------|
| **Low**      | No security controls. Vulnerable code is obvious and easy to exploit. |
| **Medium**   | Basic, flawed security controls that can be bypassed with minor effort. |
| **High**     | Stronger controls, closer to real-world code, but still exploitable with technique. |
| **Impossible** | Secure, best-practice implementation for comparison. |

> **Learning strategy:** Start at Low to understand the vulnerability, then progress to Medium and High to understand defensive controls and how to bypass weak ones. Read the Impossible source code to understand the correct fix.

### DAST Baseline

**DAST** stands for **Dynamic Application Security Testing**. Unlike SAST (which analyses source code statically), DAST tests a running application by sending it real HTTP requests and observing the responses — exactly as an attacker would.

DVWA will serve as the **intentionally vulnerable target** for all future DAST exercises in this programme.

#### Why a Known-Vulnerable Target Matters for DAST

A DAST scanner (such as OWASP ZAP) is only useful if it can be verified to be working correctly. DVWA provides a ground truth:

- We know it contains SQL Injection. If the scanner does not find it, the scanner is misconfigured or the scan policy is too restrictive.
- We know it contains Stored XSS. If the scanner misses it, we can tune the scan rules and try again.
- This process of "validating the validator" is called **tool qualification** and is a standard practice in mature DevSecOps programmes.

## Evidence

### DVWA Running in Docker

![DVWA Running via Docker](Evidence/step2.1-DVWA-Docker.png)

### DVWA Login page

![DVWA Dashboard](Evidence/step2.2-login-dvwa.png)

### Create Database DVWA

![DVWA Dashboard](Evidence/step2.3-create-db-dvwa.png)

### DVWA Dashboard Post-Login

![DVWA Dashboard](Evidence/step2.4-dvwa-dashboard.png)

## Reflection

- Step 1 and Step 2 established the foundational environment for the entire DevSecOps. The key insight from these steps is that **infrastructure-as-code** — even at the smallest scale of a single `docker-compose.yml` file brings enormous benefits (reproducibility, documentation, and version control). 
- The same principles that apply to this five-line Compose file apply to a thousand-line Kubernetes Helm chart in production.
- Deploying DVWA highlighted a core tension in security engineering: the most effective way to learn how to defend against an attack is to understand how to perform it. DVWA provides a safe, legal, and structured environment to develop this understanding without any real-world harm.
- The ARM architecture warning was a valuable early lesson: the gap between a developer's local machine and the production environment is real, and Docker for all its portability benefits does not eliminate it entirely. Architecture, OS version, kernel capabilities, and available resources all differ between environments, and a mature DevSecOps practice accounts for these differences explicitly.

---
# Step 3 – GitHub

## Objective

DevSecOps lab project will use personal GitHub repository in preparation for implementing GitHub Actions, automated security scanning, and CI/CD pipelines. Avoid any permission issue may arise from GitHub Enterprise.

## Creating a New Repository

A new GitHub repository was created.

Repository:

```
https://github.com/AdaniKamal/dso-lab-dvwa

```

> The repository will be used throughout this DevSecOps learning programme.

## Command

### Initialising Git

Inside the project folder:

```bash
git init
git branch -M main
```

This created a new Git repository and renamed the default branch to `main`.

### Connecting to GitHub

The remote repository was configured.

```bash
git remote add origin https://github.com/AdaniKamal/dso-lab-dvwa.git
```

Verified using:

```bash
git remote -v
```

### First Commit

All project files were staged.

```bash
git add .
git commit -m "Initial DevSecOps lab setup"
```

The initial commit now tracks the entire project.

A `.gitignore` file was created to prevent future uploads of:

- actions-runner/
- .env
- private keys
- macOS metadata

## Problems Encountered

### Issue 2 — Empty Repository

Git initially reported:

```
nothing to commit
```

#### Cause

Git had been initialised inside an empty directory rather than the actual project directory.

#### Resolution

The project files were moved into the correct repository before committing.

### Issue 3 — Remote Already Contained Files

The first push failed.

```
! [rejected]
fetch first
```

#### Cause

The GitHub repository already contained an automatically generated README.md.

The local repository had a different Git history.

### Issue 4 — Divergent Branches

Attempting to pull produced:

```
fatal:
Need to specify how to reconcile divergent branches.
```

#### Resolution

A merge pull was used.

```bash
git pull origin main --allow-unrelated-histories --no-rebase
```

### Issue 5 — README Merge Conflict

Git reported:

```
CONFLICT (add/add):
README.md
```

Both repositories contained different README files.

#### Resolution

The README was manually rewritten.

```bash
git add README.md
git commit -m "Resolve README merge conflict"
```

## Successful Push

The repository was successfully pushed.

```bash
git push -u origin main
```

GitHub now contains the latest version of the project.

## Multiple empty folder created

The repository was successfully pushed.

```bash
# Create all folders
mkdir docs/.gitkeep Evidence/.gitkeep scans/.gitkeep

# Add .gitkeep to each folder
touch docs/.gitkeep Evidence/.gitkeep scans/.gitkeep

# Stage all at once
git add docs/.gitkeep Evidence/.gitkeep scans/.gitkeep

# Commit
git commit -m "Add empty folders: docs, Evidence, scans"

# Push
git push
```

GitHub now contains the latest version of the project.

## Evidence

### DVWA up in Docker

![DVWA Running via Docker](Evidence/step3.1-dvwa-up.png)

#### Create empty folder use gitkeep

![Create empty folder gitkeep](Evidence/step3.2-create-empty-folder-gitkeep.png)

#### Create multiple empty folder use gitkeep

![Create multiple empty folders gitkeep](Evidence/step3.3-multiple-empty-folder-gitkeep.png)

## Reflection

This migration highlighted several important Git concepts:

- Git repositories should never contain another Git repository unless using submodules.
- Enterprise GitHub environments may impose restrictions on GitHub Actions and self-hosted runners.
- Divergent Git histories require merging before pushing.
- Merge conflicts are normal when two repositories contain different versions of the same file.
- `.gitignore` is essential for preventing temporary files, secrets, and large binaries from being committed.

---
# Step 4 – GitHub Actions Workflow

## Objective

Verify that GitHub Actions is correctly configured by creating and executing the first Continuous Integration (CI) workflow using GitHub-hosted runners.

This serves as the foundation for all subsequent DevSecOps automation, including code quality checks, secret scanning, Static Application Security Testing (SAST), Software Composition Analysis (SCA), container image scanning, and Dynamic Application Security Testing (DAST).

## Why GitHub Actions?

GitHub Actions is GitHub's native Continuous Integration and Continuous Deployment (CI/CD) platform. It allows developers to automate repetitive tasks whenever specific events occur within a repository.

Typical DevSecOps tasks include:

- Building applications
- Running automated tests
- Executing code quality checks
- Detecting leaked secrets
- Performing security scans
- Deploying applications
- Generating security reports

Instead of performing these tasks manually, GitHub Actions executes them automatically whenever code is pushed to the repository.

## Workflow Architecture

![Docker Desktop Installation](Evidence/Github_Actions.png)

## Workflow File

A new workflow was created inside:

```
.github/workflows/devsecops-pipeline.yml
```

The workflow is triggered whenever code is pushed to the `main` branch.

Current workflow:

```yaml
name: DevSecOps Pipeline

on:
  push:
    branches:
      - main

jobs:

  hello-world:

    runs-on: ubuntu-latest

    steps:

      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Print Environment
        run: |
          echo "Hello DevSecOps!"
          echo "Running on:"
          uname -a

      - name: Show Files
        run: ls -lah
```

## Understanding the Workflow

### Trigger

```yaml
on:
  push:
    branches:
      - main
```

The workflow executes automatically every time new code is pushed to the `main` branch. This event-driven approach is a core concept in CI/CD pipelines.

### Job

```yaml
jobs:
  hello-world:
```

A workflow can contain multiple jobs. Currently only one job exists:

- `hello-world`

Future jobs will include:

- Super-Linter
- Gitleaks
- Semgrep
- Trivy
- OWASP ZAP

### Runner

```yaml
runs-on: ubuntu-latest
```

The workflow executes on a GitHub-hosted Ubuntu virtual machine.

Unlike the enterprise GitHub environment previously used, no self-hosted runner configuration is required. GitHub automatically provisions a fresh virtual machine for each workflow execution and removes it once the job completes.

Benefits include:

- Clean execution environment
- Consistent operating system
- No maintenance of build servers
- No local machine dependency

### Workflow Steps

The workflow currently performs three simple tasks.

#### 1. Checkout Repository

```yaml
uses: actions/checkout@v4
```

Downloads the latest repository contents into the runner. Without this step, subsequent jobs cannot access the project files.

#### 2. Print Environment

```bash
echo "Hello DevSecOps!"
uname -a
```

Purpose:
- Verify command execution
- Display operating system information
- Confirm the workflow is executing successfully

#### 3. Show Repository Files

```bash
ls -lah
```

Lists all files available inside the GitHub runner. This confirms the repository was checked out successfully.

## Execution Result

After committing and pushing the workflow file:

```bash
git add .
git commit -m "Add first GitHub Actions workflow"
git push
```

GitHub automatically detected the workflow and executed it. The workflow completed successfully.

Observed result:

```
✓ Checkout Repository
✓ Print Environment
✓ Show Files
✓ Workflow Completed Successfully
```

This confirms:

- GitHub Actions is enabled
- Workflow syntax is valid
- GitHub-hosted runners are functioning correctly
- The repository is ready for CI/CD automation

## Evidence

### Github Action

![First Github Action](Evidence/step4-First-Github-Action.png)

## Reflection

This exercise demonstrated several important CI/CD concepts:

- GitHub Actions workflows are event-driven.
- Each workflow executes inside a temporary virtual machine.
- Every workflow starts from a clean environment.
- The repository must be checked out before accessing project files.
- Simple workflows should be validated before introducing complex security tools.

> Establishing a working CI environment first simplifies troubleshooting, ensuring that future failures originate from the security tools themselves rather than the underlying CI platform.

---
# Step 5 – Super-Linter (CI Quality Gate)

## Objective

Integrate **Super-Linter** into the GitHub Actions pipeline to automatically validate source code quality before introducing additional DevSecOps security tools.

The goal of this exercise was not only to execute Super-Linter successfully, but also to understand how automated code quality checks become the first quality gate within a Continuous Integration (CI) pipeline.

## Super-Linter

Super-Linter is an open-source GitHub Action maintained by GitHub that combines multiple language-specific linters into a single workflow.

Rather than checking only one programming language, Super-Linter automatically detects supported file types within a repository and executes the appropriate linter for each file.

Examples include:

| File Type | Linter |
|------------|---------|
| YAML | yamllint |
| Markdown | markdownlint |
| JSON | jsonlint |
| Dockerfile | Hadolint |
| Shell Script | ShellCheck |
| Python | Pylint |
| JavaScript | ESLint |

This allows developers to maintain consistent code quality using a single GitHub Actions workflow.

### Why Super-Linter Matters in DevSecOps

Super-Linter represents the **first automated quality gate** in the DevSecOps pipeline.

Instead of allowing incorrectly formatted configuration files or source code to progress further through the CI/CD pipeline, the workflow immediately reports issues and stops execution.

This provides several benefits:

- Detects syntax errors early
- Enforces coding standards
- Prevents malformed configuration files from reaching production
- Reduces debugging time
- Improves repository consistency

In mature DevSecOps environments, code quality validation occurs before security scanning or deployment.

> Only after this stage passes should later security tools execute.

## Workflow Configuration

The workflow file is located at:

```
.github/workflows/devsecops-pipeline.yml
```

Current configuration:

```yaml
name: DevSecOps Pipeline

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  super-linter:

    runs-on: ubuntu-latest

    permissions:
      contents: read
      packages: read
      statuses: write

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Run Super-Linter
        uses: super-linter/super-linter@v8

        env:
          DEFAULT_BRANCH: main
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}

          VALIDATE_ALL_CODEBASE: true
          VALIDATE_YAML: true
```

## Why Only YAML Was Validated

Although Super-Linter supports many languages, this stage intentionally validates only YAML files.

Reasons:

- GitHub Actions workflows themselves are YAML.
- Docker Compose files are YAML.
- Kubernetes manifests are YAML.
- CI/CD pipelines frequently depend on YAML configuration.

By limiting validation to YAML during the initial implementation, troubleshooting became significantly easier before enabling additional language validators.

## Problems Encountered

Several issues were encountered while integrating Super-Linter.

### 1. Repository History Error

Initial workflow execution failed with:

```
Failed to validate GITHUB_BEFORE_SHA
```

#### Cause

The repository had recently been migrated from an enterprise GitHub environment and contained unrelated Git histories after merging repositories.

Super-Linter attempted to compare commits that were unavailable in the shallow Git checkout.

#### Resolution

The checkout action was modified to retrieve the complete repository history.

```yaml
with:
    fetch-depth: 0
```

### 2. Mixed Validation Configuration

Another workflow execution failed with:

```
Behavior not supported,
please either only include
(VALIDATE=true)

or exclude

(VALIDATE=false)
```

#### Cause

Super-Linter does not allow combining included and excluded validators simultaneously.

Example:

```yaml
VALIDATE_YAML: true
VALIDATE_MARKDOWN: false
```

This configuration is invalid.

#### Resolution

The workflow was simplified to validate only YAML.

### 3. Intentional YAML Validation

A test YAML file was created to verify that Super-Linter successfully validates YAML syntax.

Example:

```yaml
name: Dani
age: 26
```

The file passed validation successfully.

## Successful Execution

After correcting the workflow configuration, GitHub Actions completed successfully.

Observed result:

```
✓ Checkout Repository
✓ Run Super-Linter
✓ YAML Validation Passed
✓ Workflow Completed Successfully
```

GitHub generated the following summary:

| Language | Result |
|----------|--------|
| YAML | ✅ Pass |

All YAML files were validated successfully.

## Evidence

### Failed Super Linter

![Failed Super-Linter](Evidence/step5.1-Failed-Super-Linter.png)

*GitHub Actions workflow showing failed Super-Linter execution.*

### Finally success Super Linter

![Success Super-Linter](Evidence/step5.2-Finally-Success-SL.png)

*Super-Linter summary page showing successful YAML validation.*

### Success Super Linter

![Success Super-Linter Full](Evidence/step5.3-Success-Super-Linter.png)

## Reflection

This exercise demonstrated several important DevSecOps concepts.

- GitHub Actions executes workflows inside temporary cloud-hosted runners.
- Super-Linter automatically selects the appropriate linter for supported file types.
- Repository history may affect certain GitHub Actions workflows.
- Quality gates should be implemented before introducing security scanning tools.
- YAML validation is especially important because modern CI/CD platforms rely heavily on YAML-based configuration.
- This stage demonstrated that Continuous Integration (CI) is not solely concerned with building or deploying applications. 
- An effective CI pipeline also acts as an automated reviewer, ensuring that configuration files meet defined quality standards before additional security scans or deployment activities occur.
- Successfully integrating Super-Linter establishes the first quality gate within the DevSecOps pipeline and provides a reliable foundation for introducing secret scanning, static application security testing, dependency analysis, and dynamic application security testing in subsequent stages.

# Step 6 – Gitleaks (Secret Scanning)

Integrate **Gitleaks** into the GitHub Actions pipeline to automatically detect hardcoded secrets committed into the repository.

## Objective

The objective of this stage is to prevent sensitive credentials such as passwords, API keys, access tokens, and private keys from being accidentally committed into source control.

## Gitleaks

Gitleaks is an open-source **Secret Detection** tool that scans Git repositories for exposed credentials.

Rather than scanning applications for vulnerabilities, Gitleaks searches source code, configuration files, and Git commit history for strings that resemble secrets.

Examples include:

| Secret Type | Example |
|-------------|---------|
| AWS Access Key | `AKIA...` |
| GitHub Personal Access Token | `ghp_...` |
| Azure Keys | Azure Storage Keys |
| Google API Keys | `AIza...` |
| SSH Private Keys | `-----BEGIN OPENSSH PRIVATE KEY-----` |
| Database Passwords | Hardcoded credentials |
| JWT Tokens | JSON Web Tokens |
| Generic API Keys | Vendor-specific secrets |

## The important

One of the most common causes of cloud breaches is the accidental exposure of credentials in Git repositories. Once secrets are committed, they may remain accessible through Git history even after being removed from the latest version of the file. 

Attackers continuously scan public repositories looking for exposed credentials.

A leaked credential may allow an attacker to:

- Access cloud infrastructure
- Download sensitive data
- Deploy malicious resources
- Access production databases
- Escalate privileges

Because of this, secret scanning should occur before any deployment stage.

> Gitleaks verifies that no secrets have been committed.

## Workflow Configuration

The workflow was updated to include a second GitHub Actions job.

```yaml
gitleaks:
  name: Gitleaks Secret Scan
  runs-on: ubuntu-latest
  needs: super-linter

  permissions:
    contents: read
    pull-requests: read

  steps:

    - name: Checkout Repository
      uses: actions/checkout@v6
      with:
        fetch-depth: 0

    - name: Run Gitleaks
      uses: gitleaks/gitleaks-action@v3
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Understanding the Workflow

### Job Dependency

```yaml
needs: super-linter
```

This ensures that Gitleaks will only execute after Super-Linter completes successfully.

If Super-Linter fails, Gitleaks will not run. This demonstrates how multiple jobs can be chained together within a CI pipeline.

### Repository Checkout

```yaml
fetch-depth: 0
```

A complete Git history is retrieved before scanning.

This allows Gitleaks to inspect not only the latest files but also previous commits where secrets may have existed.

### GitHub Token

```yaml
GITHUB_TOKEN
```

GitHub automatically generates a temporary authentication token for every workflow execution.

This token allows GitHub Actions to securely interact with the repository without exposing personal credentials.

## Successful Execution

After updating the workflow and pushing the changes:

```bash
git add .
git commit -m "Add Gitleaks secret scanning"
git push
```

GitHub Actions executed both jobs successfully.

Observed pipeline:

```
✓ Super-Linter
✓ Gitleaks
✓ Workflow Completed Successfully
```

No exposed secrets were detected within the repository.

## Evidence

### Screenshot 1

![Gitleaks Success](Evidence/step6-gitleaks-success.png)

*GitHub Actions workflow showing successful execution of the Gitleaks secret scanning job.*

## Reflection

This stage introduced several important DevSecOps concepts.

- Secret scanning is different from vulnerability scanning.
- Gitleaks searches for exposed credentials rather than software vulnerabilities.
- Git history should also be scanned because deleted secrets may still exist in previous commits.
- Job dependencies (`needs`) allow security checks to execute in a controlled sequence.
- Secret detection should occur before build and deployment stages.
- This stage marks the transition from **code quality validation** to **security validation**.
- Gitleaks protects the software supply chain by detecting accidentally committed secrets before they can be propagated further. 
- Integrating Gitleaks early in the pipeline helps reduce the risk of credential exposure and reinforces secure development practices throughout the software development lifecycle.

---
# Step 7 – Semgrep (Static Application Security Testing)

## Objective

Integrate **Semgrep** into the GitHub Actions pipeline to perform **Static Application Security Testing (SAST)** against the repository.

Unlike previous stages that focused on code quality (Super-Linter) and exposed credentials (Gitleaks), Semgrep analyzes source code and configuration files for insecure coding patterns and security misconfigurations.

## What is SAST?

Static Application Security Testing (SAST) is a security testing methodology that analyzes an application's source code without executing it.

Instead of attacking a running application, SAST examines the code itself to identify potential security weaknesses before deployment.

Typical vulnerabilities detected include:

- SQL Injection
- Command Injection
- Hardcoded credentials
- Insecure deserialization
- Weak cryptography
- Dangerous API usage
- Security misconfigurations

SAST is considered a **shift-left** security practice because vulnerabilities are identified during development rather than after deployment.

## Semgrep

Semgrep is an open-source Static Application Security Testing (SAST) tool.

It scans source code using predefined and custom security rules to identify insecure coding patterns.

Unlike traditional SAST tools that often require lengthy configuration, Semgrep provides lightweight rule-based scanning and supports many programming languages.

Supported technologies include:

- Python
- Java
- JavaScript
- TypeScript
- Go
- Docker
- YAML
- Kubernetes
- Terraform
- GitHub Actions

## Position in the DSO Pipeline

The current pipeline now consists of three validation stages.

Each stage performs a different responsibility.

| Tool | Purpose |
|-------|----------|
| Super-Linter | Code quality validation |
| Gitleaks | Secret detection |
| Semgrep | Static security analysis |

## Workflow Configuration

A third GitHub Actions job was added.

```yaml
semgrep:
  name: Semgrep SAST Scan

  runs-on: ubuntu-latest

  needs: gitleaks

  permissions:
    contents: read

  steps:

    - name: Checkout Repository
      uses: actions/checkout@v6
      with:
        fetch-depth: 0

    - name: Install Semgrep
      run: pipx install semgrep

    - name: Run Semgrep
      run: semgrep scan --config auto
```

## Understanding the Workflow

### Job Dependency

```yaml
needs: gitleaks
```

Semgrep executes only after the Gitleaks scan completes successfully.

This establishes a sequential security pipeline:

```
Super-Linter
      │
      ▼
Gitleaks
      │
      ▼
Semgrep
```

### Automatic Rule Selection

```bash
semgrep scan --config auto
```

The `auto` configuration automatically selects suitable rules based on the technologies detected within the repository.

In this project, Semgrep scanned:

- GitHub Actions workflow files
- YAML configuration files

As the repository grows, Semgrep will automatically include rules for additional languages such as Python or Dockerfiles.

## Initial Findings

During the first execution, Semgrep detected five security findings.

The findings were not application vulnerabilities but **GitHub Actions supply-chain recommendations**.

Examples included:

```
uses: actions/checkout@v6
uses: super-linter/super-linter@v8
uses: gitleaks/gitleaks-action@v3
```

Semgrep recommended pinning each GitHub Action to an immutable commit SHA instead of using mutable version tags.

## Why This Was Reported

Version tags such as:

```yaml
uses: actions/checkout@v6
```

Are mutable references. Although `v6` currently points to a trusted release, repository maintainers can move the tag to a different commit in the future.

Using a full commit SHA ensures the workflow always executes the exact version that has been reviewed and approved.

Example:

```yaml
uses: actions/checkout@8ade135a41bc03ea155e62e844d188df1ea18608
```

This practice strengthens software supply-chain security by preventing unexpected updates to GitHub Actions.

## Trade-offs

Initially, Semgrep was executed using:

```bash
semgrep scan --config auto --error
```

The `--error` option causes the workflow to fail whenever blocking findings are detected.

Although this reflects production security practices, the workflow was adjusted for this learning project to:

```bash
semgrep scan --config auto
```

This allows findings to be reported without stopping the pipeline, enabling the focus to remain on understanding the results while progressively building the DevSecOps pipeline.

Future iterations of the pipeline will re-enable blocking mode after workflow hardening is complete.

## Successful Execution

After updating the workflow configuration, the GitHub Actions pipeline completed successfully.

Pipeline status:

```
✓ Super-Linter
✓ Gitleaks
✓ Semgrep
✓ Workflow Completed Successfully
```

Semgrep successfully analyzed the repository and generated security findings without interrupting the pipeline.

## Evidence

### Semgrep success

![Semgrep Success](Evidence/step7-semgrep-success.png)

*GitHub Actions showing successful execution of the Semgrep SAST job.*

### How to find ref tags

![Find Ref Tags](Evidence/step7.1-find-ref-tags.png)

## Reflection

This exercise introduced several important DevSecOps concepts.

- SAST analyzes source code without executing the application.
- Semgrep performs rule-based security analysis.
- GitHub Actions workflows themselves can be security scanning targets.
- Software supply-chain security includes securing CI/CD workflows.
- Mutable GitHub Action tags may introduce supply-chain risks.
- Security findings should be reviewed and triaged before deciding whether they should block the pipeline.
- This stage expanded the pipeline from syntax and secret validation into **static security analysis**. 
- Semgrep evaluated the repository for security-related coding patterns and workflow configuration issues.
- The initial findings also demonstrated that modern DevSecOps extends beyond application code. Security tooling increasingly evaluates the CI/CD pipeline itself, emphasizing software supply-chain security and secure automation practices.

---
# Step 8 – Trivy Filesystem Scan (Software Composition Analysis)

## Objective

Integrate **Trivy** into the GitHub Actions pipeline to perform **Software Composition Analysis (SCA)** against the project repository.

Unlike Semgrep, which analyzes source code for insecure coding patterns, Trivy focuses on identifying known vulnerabilities within project dependencies, configuration files, Infrastructure-as-Code (IaC), and software components.

The objective of this stage is to identify vulnerable third-party software before deployment.

## Software Composition Analysis (SCA)

Modern applications rely heavily on third-party libraries, frameworks, and open-source packages.Rather than developing every component from scratch, developers import external dependencies into the applications.

While this accelerates software development, it also introduces security risks.

A vulnerable dependency may expose the application even if the developer's own code is secure.

Software Composition Analysis (SCA) automatically identifies:

- Vulnerable libraries
- Outdated packages
- Known CVEs
- License issues
- Supply-chain risks

This helps development teams detect vulnerable components early within the CI/CD pipeline.

## What is Trivy?

Trivy is an open-source security scanner developed by Aqua Security.

It is capable of scanning multiple targets including:

| Scan Target | Description |
|-------------|-------------|
| Filesystem | Source code and project dependencies |
| Container Images | Docker images |
| Kubernetes | Cluster resources |
| Terraform | Infrastructure-as-Code |
| Secrets | Hardcoded credentials |
| Misconfiguration | Security configuration issues |
| SBOM | Software Bill of Materials |

Because of its versatility, Trivy is commonly integrated into modern DevSecOps pipelines.

## Position in the DevSecOps Pipeline

The pipeline now contains four automated security stages. Each tool performs a different security function.

| Tool | Security Function |
|-------|-------------------|
| Super-Linter | Code Quality |
| Gitleaks | Secret Detection |
| Semgrep | Static Application Security Testing (SAST) |
| Trivy | Software Composition Analysis (SCA) |

## Workflow Configuration

A fourth GitHub Actions job was added.

```yaml
trivy-fs:
  name: Trivy Filesystem Scan
  runs-on: ubuntu-latest
  needs: semgrep
  permissions:
    contents: read
  steps:
    - name: Checkout Repository
      uses: actions/checkout@<Pinned Commit SHA>
    - name: Run Trivy Filesystem Scan
      uses: aquasecurity/trivy-action@<Pinned Commit SHA>
      with:
        scan-type: fs
        scan-ref: .
        format: table
        exit-code: "0"
        severity: CRITICAL,HIGH
```

## Understanding the Workflow

### Job Dependency

```yaml
needs: semgrep
```

The Trivy scan executes only after Semgrep completes successfully.

This ensures the pipeline follows a layered security approach.

```
Super-Linter
      │
      ▼
Gitleaks
      │
      ▼
Semgrep
      │
      ▼
Trivy
```

### Filesystem Scan

```yaml
scan-type: fs
```

Instead of scanning a Docker image, Trivy scans the repository itself.

This includes:

- Project files
- Dependency manifests
- Configuration files
- Infrastructure-as-Code
- Security policies

Filesystem scanning is typically performed before container image scanning.

### Repository Target

```yaml
scan-ref: .
```

The current repository directory is scanned.

As the project grows, Trivy will automatically analyze supported package managers such as:

- Python (requirements.txt)
- Node.js (package-lock.json)
- Java (pom.xml)
- Go (go.mod)
- Rust (Cargo.lock)

### Severity Filter

```yaml
severity: CRITICAL,HIGH
```

Only High and Critical vulnerabilities are reported.

This reduces unnecessary noise while focusing on vulnerabilities that require immediate attention.

### Exit Code

```yaml
exit-code: "0"
```

Although Trivy reports vulnerabilities, it does not fail the pipeline during this learning phase.

Future iterations of the pipeline may use:

```yaml
exit-code: "1"
```

to enforce blocking security gates.

## Workflow Hardening

During implementation, Semgrep reported a supply-chain security finding against the Trivy GitHub Action.

Example:

```yaml
uses: aquasecurity/trivy-action@0.33.1
```

Semgrep recommended replacing mutable version tags with immutable Git commit SHAs.

The workflow was updated using the commit SHA obtained from:

```bash
git ls-remote https://github.com/aquasecurity/trivy-action.git refs/tags/v0.33.1
```

This aligns with GitHub Actions security best practices by preventing unexpected changes to third-party actions.

## Successful Execution

After updating the workflow and pinning the Trivy GitHub Action, the pipeline completed successfully.

Observed pipeline:

```
✓ Super-Linter
✓ Gitleaks
✓ Semgrep
✓ Trivy Filesystem
✓ Workflow Completed Successfully
```

The workflow graph confirmed that all four security stages executed sequentially.

---
## Evidence

### Screenshot 1

![Trivy Filesystem Success](Evidence/step8-trivy-success.png)

*GitHub Actions workflow showing successful execution of the Trivy Filesystem Scan job.*

### Screenshot 2

![Trivy Latest Version](Evidence/step8.1-trivy-latest-version.png)

*Retrieving the Git commit SHA for the Trivy GitHub Action using `git ls-remote`, demonstrating immutable action pinning.*

## Reflection

This exercise introduced several important DevSecOps concepts.

- Software Composition Analysis differs from Static Application Security Testing.
- Vulnerabilities may originate from third-party dependencies rather than application code.
- Trivy supports multiple scan targets beyond container images.
- Security scanning should occur before software packaging and deployment.
- GitHub Actions workflows themselves should follow supply-chain security best practices by pinning actions to immutable commit SHAs.
- Multiple security tools can be chained together to create layered security validation.
- This stage extended the DevSecOps pipeline beyond source code analysis into dependency security.
- The workflow also introduced an important supply-chain security concept by replacing mutable GitHub Action version tags with immutable Git commit SHAs. 
- This reduces the risk of unintended updates to third-party workflow components and strengthens the overall security posture of the CI/CD pipeline.

---
# Step 9 – Containerization with Docker Compose

## Objective

Prepare the Damn Vulnerable Web Application (DVWA) for containerized deployment using **Docker Compose**.

The objective of this stage is to establish a reproducible application environment that can later be scanned by security tools such as Trivy and OWASP ZAP.

Rather than manually installing Apache, PHP, and MySQL, Docker Compose automates the deployment of the entire application stack.

---
## Why Docker?

Containerization has become the de facto deployment method for modern applications.

Instead of configuring software directly on an operating system, Docker packages the application together with all required dependencies into a portable container.

Benefits include:

- Consistent environments
- Faster deployment
- Simplified dependency management
- Easy rollback
- Platform independence

This ensures the application behaves consistently across development, testing, and production environments.

---
## Why Docker Compose?

While Docker runs a single container, Docker Compose manages one or more related containers using a YAML configuration file. Compose allows infrastructure to be defined as code.

Instead of executing long Docker commands manually, the application can be started using:

```bash
docker compose up -d
```

and stopped using:

```bash
docker compose down
```

This improves repeatability and automation.

## Docker Compose Configuration

The project was updated with a new deployment configuration.

**File**

```
docker-compose.yml
```

Configuration:

```yaml
services:
  dvwa:
    image: vulnerables/web-dvwa
    container_name: dvwa

    ports:
      - "127.0.0.1:8080:80"

    restart: unless-stopped
```

## Understanding the Configuration

### Service Name

```yaml
services:
  dvwa:
```

Defines a containerized service named **dvwa**.

### Docker Image

```yaml
image: vulnerables/web-dvwa
```

The application is deployed using the official DVWA Docker image published on Docker Hub.

This removes the need to manually install Apache, PHP, and other dependencies.

### Container Name

```yaml
container_name: dvwa
```

Assigns a fixed container name for easier administration and scripting.

Instead of referencing Docker-generated IDs, the container can be managed directly using:

```bash
docker stop dvwa
docker start dvwa
docker logs dvwa
```

### Port Mapping

```yaml
ports:
  - "127.0.0.1:8080:80"
```

Maps:

| Host | Container |
|------|-----------|
| 127.0.0.1:8080 | 80 |

The application is only exposed on the local machine. Using `127.0.0.1` instead of `0.0.0.0` prevents external hosts from accessing the application.

This is appropriate because DVWA is intentionally vulnerable and intended solely for security testing.

### Restart Policy

```yaml
restart: unless-stopped
```

The container automatically restarts after a reboot unless it has been explicitly stopped by the user. This improves availability during development.

## Deployment

The application was deployed using Docker Compose.

Start the application:

```bash
docker compose up -d
```

Verify the running container:

```bash
docker ps
```

Access the application:

```
http://localhost:8080
```

Stop the application:

```bash
docker compose down
```

## Why DVWA?

DVWA (Damn Vulnerable Web Application) is an intentionally insecure web application designed for security education.

It includes common web vulnerabilities such as:

- SQL Injection
- Cross-Site Scripting (XSS)
- Command Injection
- File Inclusion
- CSRF
- Weak Authentication

These vulnerabilities provide realistic targets for security testing tools.
Later stages of this project will use DVWA as the target application for automated security scanning.

## Role in the DevSecOps Pipeline

This stage prepares the application for security testing.

```
Developer
      │
      ▼
Git Push
      │
      ▼
GitHub Actions
      │
      ├── Super-Linter
      ├── Gitleaks
      ├── Semgrep
      ├── Trivy Filesystem
      │
      ▼
Docker Compose
      │
      ▼
DVWA Container
```

The application is now ready for container image scanning and dynamic security testing.

## Evidence

### Screenshot 1

![Docker Compose Config](Evidence/step9-docker-compose.png)

*Docker Compose configuration defining the DVWA deployment.*

### Screenshot 2

![Docker Compose Up](Evidence/step9.1-docker-compose.png)

*Successful deployment of the DVWA container using Docker Compose.*

### Screenshot 3

![Docker PS](Evidence/step9.2-docker-ps.png)

*Verification that the DVWA container is running.*

## Reflection

This exercise introduced several important DevSecOps concepts.

- Infrastructure can be defined as code using Docker Compose.
- Containers provide consistent deployment environments.
- Docker Compose simplifies multi-container management.
- Localhost binding reduces unnecessary exposure of intentionally vulnerable applications.
- Containerization forms the foundation for automated security testing within CI/CD pipelines.
- By containerizing DVWA with Docker Compose, the deployment process becomes repeatable, portable, and suitable for integration into a CI/CD pipeline. 
- This containerized environment serves as the foundation for the next stages of the project, where the Docker image will be analyzed using Trivy and the running application will be evaluated using OWASP ZAP.

---
# Step 10 – Trivy Container Image Scan

## Objective

Integrate **Trivy Container Image Scanning** into the GitHub Actions pipeline to analyze the Docker image used by the Damn Vulnerable Web Application (DVWA).

Unlike the previous Trivy Filesystem scan, which examined the repository contents, this stage evaluates the security posture of the Docker container itself by identifying vulnerabilities within the operating system packages and application components bundled inside the image.

The objective is to ensure that container images are assessed for known vulnerabilities before deployment.

## Why Scan Docker Images?

Modern applications are commonly deployed using containers.

Although application source code may be secure, the container image itself contains many software packages such as:

- Linux operating system
- Apache HTTP Server
- PHP
- OpenSSL
- libc
- Bash
- System utilities

These components may contain publicly disclosed Common Vulnerabilities and Exposures (CVEs).

Therefore, container image scanning has become an essential security control within DevSecOps pipelines.

## Filesystem Scan vs Container Image Scan

Both scans use Trivy but analyse different targets.

| Trivy Scan | Target | Purpose |
|------------|--------|---------|
| Filesystem Scan | Repository | Scan project files, dependencies, configuration and Infrastructure-as-Code |
| Container Image Scan | Docker Image | Scan installed operating system packages and application libraries inside the container |

The Filesystem scan focuses on the project source, whereas the Image scan focuses on the software that will actually be deployed.

## Workflow Configuration

A new GitHub Actions job was added after the filesystem scan.

```yaml
trivy-image:
  name: Trivy Docker Image Scan

  runs-on: ubuntu-latest

  needs: trivy-fs

  permissions:
    contents: read

  steps:

    - name: Checkout Repository
      uses: actions/checkout@<Pinned Commit SHA>

    - name: Pull DVWA Image
      run: docker pull vulnerables/web-dvwa:latest

    - name: Scan Docker Image
      uses: aquasecurity/trivy-action@<Pinned Commit SHA>

      with:
        image-ref: vulnerables/web-dvwa:latest
        format: table
        severity: HIGH,CRITICAL
        exit-code: "0"
```

## Understanding the Workflow

### Job Dependency

```yaml
needs: trivy-fs
```

The Docker image scan begins only after the Filesystem scan completes successfully.

This creates a layered security workflow.

```
Super-Linter
      │
      ▼
Gitleaks
      │
      ▼
Semgrep
      │
      ▼
Trivy Filesystem
      │
      ▼
Trivy Image
```

### Pull Docker Image

```bash
docker pull vulnerables/web-dvwa:latest
```

Before scanning, GitHub Actions downloads the latest DVWA Docker image from Docker Hub.

This ensures that Trivy analyses the exact image that will later be deployed for security testing.

### Image Scan

```yaml
image-ref: vulnerables/web-dvwa:latest
```

Instead of analysing source files, Trivy inspects every software package installed inside the Docker image.

Examples include:

- Apache HTTP Server
- PHP Runtime
- Linux Base Image
- SSL Libraries
- System Packages

Each package is compared against Trivy's vulnerability database to identify known CVEs.

### Severity Filter

```yaml
severity: HIGH,CRITICAL
```

Only High and Critical vulnerabilities are reported.

This helps prioritise vulnerabilities that require immediate attention while reducing informational noise.

### Exit Code

```yaml
exit-code: "0"
```

The pipeline reports detected vulnerabilities but continues execution.

For production environments, this value can be changed to:

```yaml
exit-code: "1"
```

to prevent deployment when serious vulnerabilities are identified.

## Workflow Hardening

During implementation, Semgrep identified the Trivy GitHub Action as using a mutable version tag.

To strengthen software supply-chain security, the workflow was updated to use immutable Git commit SHAs.

The commit SHA was retrieved using:

```bash
git ls-remote https://github.com/aquasecurity/trivy-action.git refs/tags/v0.33.1
```

The resulting commit SHA was then used to pin the GitHub Action.

Using immutable references reduces the risk of unexpected changes to third-party GitHub Actions.

## Successful Execution

After updating the workflow, GitHub Actions successfully completed all pipeline stages.

Pipeline result:

```
✓ Super-Linter
✓ Gitleaks
✓ Semgrep
✓ Trivy Filesystem
✓ Trivy Docker Image
✓ Workflow Completed Successfully
```

The workflow graph confirmed that the Docker image scan executed after the Filesystem scan.

## Evidence
### Trivy image success

![Trivy Image Success](Evidence/step10-trivy-image-success.png)

*GitHub Actions workflow showing successful execution of the Trivy Docker Image Scan.*

### Trivy Workflow Graph

![Trivy Workflow Graph](Evidence/step10.1-trivy-workflow-graph.png)

*Workflow graph demonstrating the execution sequence from Super-Linter through Trivy Container Image Scan.*

## Reflection

This exercise introduced several important DevSecOps concepts.

- Docker images should be scanned before deployment.
- Secure source code alone does not guarantee a secure application.
- Operating system packages frequently contain vulnerabilities.
- Image scanning and Filesystem scanning complement each other.
- Security gates can be chained together using GitHub Actions job dependencies.
- GitHub Actions themselves should follow supply-chain security best practices through immutable action pinning.
- This stage expanded security validation from the project repository to the deployable application artifact itself. 
- While the Filesystem scan assessed the source repository and configuration files, the Container Image scan evaluated the software that will actually execute in production.

---
# Step 11 – OWASP ZAP (Dynamic Application Security Testing)

## Objective

Integrate **OWASP ZAP** into the GitHub Actions pipeline to perform **Dynamic Application Security Testing (DAST)** against a running DVWA container.

Unlike previous security tools that analyze source code or container images, OWASP ZAP scans the application while it is running by interacting with it through HTTP requests, simulating an external attacker.

## Why DAST?

Previous pipeline stages covered:

| Tool | Purpose |
|------|---------|
| Super-Linter | Code quality & syntax validation |
| Gitleaks | Secret detection |
| Semgrep | Static Application Security Testing (SAST) |
| Trivy Filesystem | Filesystem vulnerability scanning |
| Trivy Image | Container image vulnerability scanning |

OWASP ZAP complements these tools by identifying runtime web application vulnerabilities that cannot be detected through static analysis alone.

## Implementation

### Start DVWA

The workflow starts the vulnerable DVWA application using Docker Compose.

```bash
docker compose up -d
```

### Wait for Application

GitHub Actions waits until the application is reachable before starting the scan.

### Prepare Report Directory

Create a writable directory for ZAP reports.

```bash
mkdir -p zap-output
chmod 777 zap-output
```

### Execute OWASP ZAP Baseline Scan

The workflow launches the official OWASP ZAP Docker image.

```bash
docker run --rm \
  --network host \
  -v $(pwd)/zap-output:/zap/wrk \
  ghcr.io/zaproxy/zaproxy:stable \
  zap-baseline.py \
  -t http://localhost:8080 \
  -r zap-report.html
```

### Upload Scan Report

The generated HTML report is uploaded as a GitHub Actions artifact for later review.

## Issues Encountered

### 1. Permission Denied

Initially, ZAP was unable to generate the report.

```
AccessDeniedException
/zap/wrk/zap-report.html
```

**Solution**

Create a writable directory (`zap-output`) and mount it into the ZAP container.

### 2. Pipeline Failed Due to Warnings

Although ZAP completed the scan successfully, GitHub Actions failed because ZAP returned:

```
Exit Code 2
```

This indicates:

- Scan completed successfully
- Vulnerabilities were detected
- No execution errors occurred

**Solution**

Allow the workflow to continue even when warnings are found.

```bash
|| true
```

This preserves the scan results while preventing expected DVWA vulnerabilities from failing the entire CI pipeline.

## Scan Summary

The baseline scan completed successfully.

| Result | Count |
|---------|------:|
| PASS | 51 |
| WARN | 16 |
| FAIL | 0 |

Example findings included:

- Missing Content Security Policy (CSP)
- Missing X-Content-Type-Options
- Missing HttpOnly cookies
- Missing SameSite cookies
- Missing Clickjacking protection
- Server version disclosure
- Debug information disclosure

These findings are expected because DVWA is intentionally insecure.

## Evidence
### Pipeline starts DVWA container

![Pipeline DVWA Start](Evidence/step11.1-pipeline-starts-DVWA-container.png)

### OWASP ZAP DAST Success

![OWASP ZAP Success](Evidence/step11.2-OWASP-ZAP-DAST-Success.png)

## Reflection

This step demonstrates how DAST differs from previous scanning stages.

Static scanners inspect source code or container images before deployment.

OWASP ZAP evaluates the application while it is running, identifying HTTP response headers, authentication flows, cookies, exposed information, and web security misconfigurations.

This provides another layer of security validation within the DevSecOps pipeline.

# Milestone Achieved

The CI pipeline now performs security validation across multiple layers of the Software Development Lifecycle (SDLC), including:

- Source code validation
- Secret detection
- Static Application Security Testing (SAST)
- Filesystem vulnerability scanning
- Container image vulnerability scanning
- Dynamic Application Security Testing (DAST)

## DevSecOps Pipeline

At this stage, the GitHub Actions workflow successfully performs the following automated security checks:

| Stage | Tool | Status |
|--------|------|--------|
| Code Quality | Super-Linter | ✅ |
| Secret Detection | Gitleaks | ✅ |
| Static Analysis (SAST) | Semgrep | ✅ |
| Filesystem Vulnerability Scan | Trivy FS | ✅ |
| Container Image Scan | Trivy Image | ✅ |
| Dynamic Application Security Testing (DAST) | OWASP ZAP | ✅ |

---
# REFERENCE

1. https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/enabling-features-for-your-repository/managing-github-actions-settings-for-a-repository?
2. OWASP DevSecOps: https://owasp.org/www-project-devsecops/
3. GitHub Security Lab: https://securitylab.github.com/tools/codeql/
4. DevSecOps Playbook: https://www.devsecops.org/
5. Awesome DevSecOps GitHub: https://github.com/TaptuIT/awesome-devsecops
