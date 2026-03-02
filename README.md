# Jenkins CI Pipeline – Freestyle Job

## Project Overview

This project demonstrates how to create a Continuous Integration (CI) pipeline for a Java Maven application using a Jenkins Freestyle Job.

The pipeline performs the following steps:

1. Connects to a Git repository
2. Builds a JAR file using Maven
3. Builds a Docker image
4. Pushes the Docker image to a private DockerHub repository

---

## Technologies Used

- Jenkins (running inside a Docker container)
- Docker
- Linux
- Git
- Java
- Maven

---

## 1️⃣ Run Jenkins in a Docker Container

Start Jenkins:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  jenkins/jenkins:lts
```

Access Jenkins at:

```
http://localhost:8080
```

---

## 2️⃣ Make Docker Available Inside Jenkins Container

Because Jenkins is running inside Docker, it does not have Docker access by default.

Restart Jenkins with Docker socket mounted:

```bash
docker run -d \
  --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  jenkins/jenkins:lts
```

This allows Jenkins to communicate with the host Docker daemon.

> For this project, security best practices are intentionally ignored.

---

## 3️⃣ Install Required Tools Inside Jenkins Container

Enter the Jenkins container:

```bash
docker exec -it jenkins bash
```

Install Maven and Docker CLI:

```bash
apt update
apt install maven docker.io -y
```

Verify installations:

```bash
mvn -v
docker -v
```

---

## 4️⃣ Configure Jenkins Credentials

In Jenkins:

1. Go to **Manage Jenkins**
2. Click **Credentials**
3. Add:
    - Git repository credentials
    - DockerHub credentials (Username and Password)

---

## 5️⃣ Create the Freestyle Job

1. Click **New Item**
2. Enter a name
3. Select **Freestyle Project**
4. Click **OK**

---

### Source Code Management

- Select **Git**
- Enter repository URL
- Add credentials

---

### Build Step

Under **Build**, click **Add build step** → **Execute shell**

Add:

```bash
mvn clean package

docker build -t <dockerhub-username>/jma:1.0 .

echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin

docker push <dockerhub-username>/java-app:latest
```

Replace `<dockerhub-username>` with your DockerHub private repository.

Ensure `DOCKER_USER` and `DOCKER_PASS` are configured in Jenkins credentials while setting up the freestyle job.

---

## 6️⃣ CI Pipeline Flow

When the job runs:

1. Jenkins pulls the source code from Git
2. Maven builds the JAR file
3. Docker builds the image
4. Jenkins logs in to DockerHub
5. Docker image is pushed to the private repository

---

## 7️⃣ Trigger the Build

Click **Build Now** and monitor progress in **Console Output**.

---

## Project Outcome

You now have a working Jenkins Freestyle CI pipeline that:

- Automates Java application builds
- Builds Docker images
- Pushes images to a private Docker repository

This setup provides the foundation for more advanced CI/CD pipelines using Pipeline and Multibranch Pipeline jobs.