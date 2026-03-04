#!/user/bin/env groovy

library identifier: 'jenkins-shared-library@main', retriever: modernSCM([$class: 'GitSCMSource', remote: 'https://github.com/oannonye/jenkins-shared-library.git', credentials: 'github-credentials'])
def gv

pipeline {   
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage("init") {
            steps {
                script {
                    gv = load "script.groovy"
                }
            }
        }
        stage("build jar") {
            steps {
                script {
                    buildJar()
                }
            }
        }

        stage("build image and push image") {
            steps {
                script {
                    buildImage 'onyebuchia/app-store:jma-2.0'
                    dockerLogin()
                    dockerPush 'onyebuchia/app-store:jma-2.0'
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    gv.deployApp()
                }
            }
        }               
    }
} 
