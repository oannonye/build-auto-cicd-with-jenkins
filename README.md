# Jenkins CI Pipeline – Pipeline Job

## Project Overview

This project demonstrates how to create a Continuous Integration (CI) pipeline for a Java Maven application using a **Jenkins Pipeline Job**.

Unlike the Freestyle job, this pipeline is fully defined in a `Jenkinsfile` located in the root directory of the repository.

The pipeline performs the following stages:

1. Connect to Git repository
2. Build JAR using Maven
3. Build Docker image
4. Login to DockerHub
5. Push Docker image to private repository

---

## Technologies Used

- Jenkins (running inside Docker container)
- Docker
- Linux
- Git
- Java
- Maven
- Jenkinsfile (Pipeline as Code)

---

## 1️⃣ Run Jenkins in Docker

Start Jenkins:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  jenkins/jenkins:lts
```

Access Jenkins at:

```
http://<server-ip>:8080
```

---

## 2️⃣ Make Docker Available Inside Jenkins Container

Since Jenkins runs inside a container, Docker must be made available.

Restart Jenkins with Docker socket mounted:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

Enter container:

```bash
docker exec -it jenkins bash
```

Install required tools:

```bash
apt update
apt install maven docker.io -y
```

Verify:

```bash
mvn -v
docker -v
```

---

## 3️⃣ Configure Jenkins Credentials

In Jenkins:

1. Go to **Manage Jenkins**
2. Click **Credentials**
3. Add:
    - Git repository credentials
    - DockerHub username/password credentials

Use appropriate credential IDs referenced inside the `Jenkinsfile`.

---

## 4️⃣ Jenkinsfile

The pipeline logic is defined in the `Jenkinsfile` located in the root of the repository.

It contains stages for:

- Checkout
- Build (Maven)
- Docker Build
- Docker Login
- Docker Push

No build logic is configured inside Jenkins UI — everything is controlled by the Jenkinsfile.

---

## 5️⃣ Create Pipeline Job

1. Click **New Item**
2. Enter job name
3. Select **Pipeline**
4. Click **OK**

---

### Pipeline Configuration

Under **Pipeline** section:

- Definition: **Pipeline script from SCM**
- SCM: **Git**
- Repository URL: `<your-repo-url>`
- Credentials: Select configured credentials
- Branch: `*/main` (or your branch name)
- Script Path: `Jenkinsfile`

Click **Save**

---

## 6️⃣ Pipeline Execution Flow

When triggered, Jenkins will:

1. Clone repository
2. Detect Jenkinsfile
3. Execute defined pipeline stages
4. Build JAR with Maven
5. Build Docker image
6. Login to DockerHub
7. Push Docker image to private repository

All stages and logic are defined inside the Jenkinsfile.

---

## 7️⃣ Trigger the Build

Click **Build Now**

Monitor execution in:

- **Stage View**
- **Console Output**

---

## Project Outcome

This Pipeline Job:

- Uses Infrastructure as Code (Jenkinsfile)
- Provides version-controlled CI configuration
- Builds and pushes Docker images automatically
- Eliminates manual build steps in Jenkins UI

This approach is more scalable and production-ready compared to Freestyle jobs.