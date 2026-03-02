# Jenkins CI Pipeline – Multibranch Pipeline Job

## Project Overview

This setup demonstrates how to configure a **Multibranch Pipeline Job** in Jenkins for a Java Maven application.

Unlike the standard Pipeline job, a Multibranch Pipeline:

- Automatically scans the Git repository
- Detects branches containing a `Jenkinsfile`
- Creates separate pipeline jobs per branch
- Automatically builds new branches when pushed

This allows true branch-based CI/CD.

---

## Technologies Used

- Jenkins (running inside Docker container)
- Docker
- Linux
- Git
- Java
- Maven
- Jenkinsfile (already created in project root)

---

## Prerequisites

All foundational setup steps are already covered in:

- Freestyle Job README
- Pipeline Job README

That includes:

- Running Jenkins inside Docker
- Mounting `/var/run/docker.sock`
- Installing Maven and Docker inside Jenkins container
- Creating Git credentials
- Creating DockerHub credentials

⚠️ Ensure those steps are completed before proceeding.

---

## Multibranch Pipeline Concept

A Multibranch Pipeline job:

- Connects to a Git repository
- Scans all branches
- Automatically builds branches that contain a `Jenkinsfile`
- Creates individual pipeline jobs dynamically

Each branch runs using its own version of the Jenkinsfile.

---

## Create Multibranch Pipeline Job

1. Go to **New Item**
2. Enter job name (e.g., `java-app-multibranch`)
3. Select **Multibranch Pipeline**
4. Click **OK**

---

## Configure Branch Source

Under **Branch Sources**:

1. Click **Add Source**
2. Select **Git**
3. Enter:
    - Repository URL
    - Credentials (previously configured)
4. Save configuration

---

## Build Configuration

Under **Build Configuration**:

- Mode: **by Jenkinsfile**
- Script Path: `Jenkinsfile`

(Ensure the Jenkinsfile is in the root directory.)

---

## Branch Discovery

By default, Jenkins will:

- Discover all branches
- Create a sub-job for each branch
- Automatically build branches containing Jenkinsfile

You may configure:

- Branch filtering
- PR discovery (optional)

---

## Scan Repository

Click:

**Scan Multibranch Pipeline Now**

Jenkins will:

- Scan repository
- Detect branches
- Create pipeline jobs per branch

---

## How It Works

When you:

- Create a new branch
- Push it to Git
- Include a Jenkinsfile

Jenkins will:

- Detect the branch
- Automatically create a new pipeline job
- Run the pipeline defined in that branch

No manual job creation required.

---

## CI/CD Flow Per Branch

For each branch:

1. Clone branch
2. Run Maven build
3. Build Docker image
4. Login to DockerHub
5. Push Docker image

(All defined inside the Jenkinsfile.)

---

## Benefits of Multibranch Pipeline

- Fully automated branch-based CI
- No manual job duplication
- Supports feature branches
- Scalable for team environments
- Production-ready CI structure

---

## Project Outcome

With this setup, you now have:

- Freestyle Job (UI-based CI)
- Pipeline Job (Pipeline as Code)
- Multibranch Pipeline (Automated branch-based CI)

This completes a full Jenkins CI pipeline implementation using:

Jenkins + Docker + Maven + Git + Java