# Dynamically Increment Application Version in Jenkins Pipeline

## DevOps CI/CD Automation Project

This project demonstrates how to dynamically increment the application version during a Jenkins CI pipeline and manage Docker image builds and pushes while avoiding commit loops. The pipeline automates the following steps: incrementing patch versions, building a Java application, cleaning old artifacts, building Docker images with dynamic tags, pushing Docker images to a private DockerHub repository, and committing the updated version back to the Git repository. The Jenkins pipeline is configured to **not trigger on version commit** to avoid infinite build loops.

---

## Technologies Used

Jenkins  
Docker  
GitLab  
Git  
Java  
Maven

---

## Project Objectives

Dynamically increment patch version during CI pipeline  
Build Java application using Maven and clean old artifacts  
Build Docker image with dynamic version-based tag  
Push Docker image to private DockerHub repository  
Commit version update back to Git repository  
Prevent Jenkins pipeline from retriggering on commit loop

---

## Step 1: Configure CI Step – Increment Patch Version

Use a Groovy script or shell command in Jenkins to increment the patch version. 
The example below uses mvn build parse helper to increment the version and then merge it with the build number from Jenkins

```groovy
steps {
    script {
        echo 'incrementing app version...'
        sh 'mvn build-helper:parse-version versions:set \
                        -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.minorVersion}.\\\${parsedVersion.nextIncrementalVersion} \
                        versions:commit'
        def matcher = readFile('pom.xml') =~ '<version>(.+)</version>'
        def version = matcher[0][1]
        env.IMAGE_NAME = "$version-$BUILD_NUMBER"
    }

}
```



---

## Step 2: Configure CI Step – Build Java Application and Clean Old Artifacts

Use Maven to clean old artifacts and build the project:

```groovy
stage('Build Java Application') {
    steps {
        sh 'mvn clean package'
    }
}
```

This ensures the workspace contains only the latest build artifacts.

---

## Step 3: Configure CI Step – Build Docker Image with Dynamic Tag

Use the dynamically incremented version for Docker image tagging:

```groovy
stage('Build Docker Image') {
    steps {
        sh "docker build -t mydockerhubuser/myapp:${IMAGE_NAME} ."
    }
}
```

---

## Step 4: Configure CI Step – Push Docker Image to Private DockerHub Repository

Log in to DockerHub (credentials stored in Jenkins) and push the Docker image:

```groovy
stage('Push Docker Image') {
    steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
            sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
            sh "docker push mydockerhubuser/myapp:${IMAGE_NAME}"
        }
    }
}
```

---

## Step 5: Configure CI Step – Commit Version Update to Git Repository

Commit the updated version file back to the repository without retriggering the pipeline:

```groovy
stage('Commit Version Update') {
    steps {
        sh '''
        git config user.email "jenkins@email.com"
        git config user.name "Jenkins CI"
        git add .
        git commit -m "Increment application version to ${IMAGE_NAME}" || echo "No changes to commit"
        git push origin HEAD:<branch>
        '''
    }
}
```

**Important:** Configure the Jenkins job to **ignore commits made by Jenkins** to avoid commit loops. This can be done by adding `[ci skip]` in the commit message or configuring Git SCM plugin to ignore commits from Jenkins.


---

## CI/CD Workflow Summary

1. Developer pushes code to Git repository
2. Jenkins pipeline triggers CI build
3. Pipeline increments patch version
4. Java application is built and old artifacts cleaned
5. Docker image is built using the new version
6. Docker image is pushed to private DockerHub repository
7. Version file is committed back to repository without retriggering pipeline

---

## Benefits

Automates versioning in CI/CD pipelines  
Eliminates manual version updates  
Standardizes Docker image tagging based on application version  
Prevents infinite commit loops in Jenkins pipelines  
Integrates Java, Maven, Docker, and Git seamlessly

---
