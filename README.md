# Jenkins Pipeline: Go Web Application Deployment

This repository demonstrates a Jenkins pipeline for building and running a sample Go web application. The pipeline clones the source code from a Git repository, builds a Docker image, and runs the containerized application.

## Pipeline Overview

The Jenkins pipeline consists of the following key steps:
1. **Git Clone**: Clones the Go web application source code from a Git repository.
2. **Docker Build and Run**: Builds a Docker image for the application and runs it in a container.

## Pipeline Code

Below is the Jenkins pipeline script:

```groovy
pipeline {
    agent {
        label {
            label 'master'
            customWorkspace "${JENKINS_HOME}/${BUILD_NUMBER}/"
        }
    }
    environment {
        Go111MODULE='on'
    }

    stages {
        stage ('Git-Clone') {
            steps {
                git 'https://github.com/kodekloudhub/go-webapp-sample.git'
            }
        }

        stage ('Docker-Build') {
            steps {
                script {
                    image = docker.build("adminturneddevops/go-webapp-sample")
                    sh "docker run -p 8090:8000 -d adminturneddevops/go-webapp-sample"
                }
            }
        }
    }
}
```groovy
