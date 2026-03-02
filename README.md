# Deploy Jenkins Server in a Docker Container

## Project Purpose

This repository demonstrates how to deploy a Jenkins server inside a Docker container on a remote Ubuntu server.

---

## Technologies Used

- Jenkins
- Docker
- DigitalOcean
- Linux (Ubuntu)

---

## Project Description

This project covers:

- Creating an Ubuntu Droplet on DigitalOcean
- Installing Docker on the server
- Running Jenkins as a Docker container
- Exposing Jenkins UI
- Mounting Docker socket to allow Jenkins run Docker builds

---

## Step 1: Create Ubuntu Server on DigitalOcean

1. Log in to DigitalOcean
2. Create a new Droplet
3. Choose Ubuntu (latest LTS recommended)
4. Select size and region
5. Add SSH key (recommended)
6. Create Droplet

---

## Step 2: Connect to the Server

SSH into the server:

```bash
ssh USER@<Droplet-IP>
```

---

## Step 3: Verify Docker Installation

Check Docker version:

```bash
docker --version
```

If Docker is not installed, install it first before proceeding.

---

## Step 4: Run Jenkins as a Docker Container

Run the following command:

```bash
docker run -p 8080:8080 -p 50000:50000 -d \
-v jenkins_home:/var/jenkins_home \
-v /var/run/docker.sock:/var/run/docker.sock \
--name jenkins \
jenkins/jenkins:lts
```

---

## What This Command Does

### Port Mapping

- `8080` → Jenkins Web UI
- `50000` → Jenkins agent communication

### Volume Mounts

`-v jenkins_home:/var/jenkins_home`  
Creates a Docker volume to persist Jenkins data.

`-v /var/run/docker.sock:/var/run/docker.sock`  
This is a **bind mount**, not a Docker volume.

---

## What Mounting `/var/run/docker.sock` Means

Mounting:

```
/var/run/docker.sock:/var/run/docker.sock
```

Means:

1. It maps the host Docker daemon socket
2. Jenkins uses the host’s Docker engine
3. It is not stored data — it is a live connection

---

## Security Implications

This setup gives Jenkins **root-level control of the host machine**.

Anyone who can run builds in Jenkins can:

- Start privileged containers
- Mount host filesystem
- Access secrets
- Control the Docker daemon

This is effectively giving Jenkins root access to the server.

This is acceptable for learning and practice environments.

---

## Production Recommendation

For production environments, a safer approach would be:

- Remote Docker Agents
- Kubernetes-based Jenkins agents

These isolate build execution from the host.

---

## Access Jenkins UI

After container startup, Jenkins will be available at:

```
http://<Droplet-IP>:8080
```

Follow the on-screen instructions to:

- Retrieve the initial admin password
- Install suggested plugins
- Create admin user

---

## Result

You now have:

- Jenkins running inside Docker
- Persistent Jenkins data
- Docker available inside Jenkins for building images

This completes Jenkins deployment in a Docker container on a DigitalOcean Ubuntu server.