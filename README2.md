# Automated CI Pipeline with Jenkins and GitHub Webhooks

## DevOps CI Automation Project

This project demonstrates how to build an automated Continuous Integration (CI) pipeline using Jenkins that is triggered automatically whenever code changes are pushed to a GitHub repository. The project integrates Jenkins with GitHub using webhooks so that every commit automatically starts a build pipeline. The pipeline checks out the latest code, builds a Java application using Maven, and optionally builds a Docker container image.

The goal of this project is to implement a simple but practical CI workflow that demonstrates core DevOps skills including automation, source control integration, pipeline configuration, and container image creation.

---

## Technologies Used

Jenkins  
GitHub  
Git  
Docker  
Java  
Maven  
Webhooks  

---

## CI Pipeline Architecture

Developer → Push Code → GitHub Repository → GitHub Webhook → Jenkins Server → CI Pipeline → Build Application → Docker Image

Pipeline Flow Explanation:

1. A developer pushes code to the GitHub repository.
2. GitHub sends a webhook notification to the Jenkins server.
3. Jenkins automatically triggers the CI pipeline.
4. Jenkins pulls the latest code from the repository.
5. Maven builds the Java application.
6. Docker packages the application into a container image.
7. Jenkins pushes the docker image to the private repo.

---

## Project Objectives

Automate CI pipeline execution  
Trigger builds automatically on code changes  
Integrate GitHub repository with Jenkins  
Build Java application using Maven  
Build Docker container image  
Demonstrate CI automation for DevOps workflow  

---

## Step 1: Install Required Jenkins Plugins

Open the Jenkins dashboard and navigate to:

Manage Jenkins → Manage Plugins → Available Plugins

Install the following plugins:

Git Plugin  
GitHub Plugin  
GitHub Integration Plugin  
Docker Pipeline Plugin  

Restart Jenkins after installation is complete.

---

## Step 2: Generate GitHub Personal Access Token

Log into GitHub and navigate to:

Settings → Developer Settings → Personal Access Tokens → Tokens (Classic)

Click **Generate New Token** and select the following permissions:

repo  
admin:repo_hook  
workflow  

Generate the token and copy it because it will be used in Jenkins credentials.

---

## Step 3: Configure GitHub Credentials in Jenkins

Open Jenkins dashboard and navigate to:

Manage Jenkins → Credentials → Global → Add Credentials

Configure credentials as follows:

Kind: Username with password  
Username: Your GitHub username  
Password: GitHub Personal Access Token  
ID: github-credentials  

Save the credentials.

---

## Step 4: Create Jenkins Pipeline Job

From the Jenkins dashboard click **New Item**.

Enter the job name:

java-maven-ci-pipeline

Select **Pipeline** and click **OK**.

---

## Step 5: Connect Jenkins to GitHub Repository

Inside the Jenkins pipeline job configuration scroll to the **Pipeline** section.

Configure the pipeline as follows:

Pipeline Definition: Pipeline script from SCM  
SCM: Git  
Repository URL: https://github.com/yourusername/your-repository.git  
Credentials: github-credentials  
Branch: module8/webhook  

Save the configuration.

---

## Step 6: Configure GitHub Webhook

Open the GitHub repository.

Navigate to:

Settings → Webhooks → Add Webhook

Configure the webhook as follows:

Payload URL  
http://YOUR_JENKINS_SERVER:8080/github-webhook/

Content Type  
application/json

Events  
Just the push event

Click **Add Webhook**.

This configuration ensures GitHub sends a webhook notification to Jenkins whenever new code is pushed.

---

## Step 7: Configure Jenkins Build Trigger

Inside the Jenkins pipeline job configuration locate **Build Triggers**.

Enable:

GitHub hook trigger for GITScm polling

Save the configuration.

---

## Step 8: Test the CI Pipeline

Make a change in the repository and push it to GitHub.

Example commands:

git add .
git commit -m "Testing Jenkins webhook trigger"
git push origin module8/webhook

Once the code is pushed the automated pipeline process begins.

---

## CI Pipeline Workflow

1 Developer pushes code to GitHub  
2 GitHub sends webhook notification to Jenkins  
3 Jenkins automatically triggers CI pipeline  
4 Jenkins checks out the latest source code  
5 Maven builds the Java application  
6 Docker builds a container image  

---

## Example Repository Structure

```
project-root
│
├── src
│
├── Dockerfile
│
├── pom.xml
│
├── Jenkinsfile
│
└── README.md
```

---

## Benefits of This Setup

Automatic build triggering on code changes  
Faster developer feedback cycle  
Reduced manual intervention in builds  
Foundation for full CI/CD pipeline implementation  
Integration between GitHub and Jenkins  

---

