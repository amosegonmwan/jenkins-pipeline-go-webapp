# Jenkins Pipeline: Go Web Application Deployment

This repository demonstrates a Jenkins pipeline for building and running a sample Go web application. The pipeline clones the source code from a Git repository, builds a Docker image, and runs the containerized application.

### Pipeline Overview

The Jenkins pipeline consists of the following key steps:
1. **Git Clone**: Clones the Go web application source code from a Git repository.
2. **Docker Build and Run**: Builds a Docker image for the application and runs it in a container.

### Pipeline Code

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
```

### How It Works
1 **Agent Configuration**
The pipeline runs on a Jenkins agent labeled `master` and uses a custom workspace determined by the Jenkins home directory and build number.

2 **Environment Variables**
* `Go111MODULE` is set to on to enable Go modules.

3 **Git-Clone Stage**
The pipeline clones the Go application source code from the following Git repository: `https://github.com/kodekloudhub/go-webapp-sample`

4 **Docker-Build Stage**
* **Builds the Docker Image:** A Docker image is built with the tag `adminturneddevops/go-webapp-sample`.
* **Runs the Container:** The application is run in a Docker container on port `8090`, mapping to the internal port `8000`.
  
### Prerequisites
* **Jenkins:** Ensure Jenkins is installed and configured.
* **Docker:** Docker must be installed on the Jenkins agent.
* **Git:** Git must be installed on the Jenkins agent.
* **Go:** Go programming language installed if modifications to the application are required.

### Accessing the Application
* Once the pipeline is executed successfully:

* The application will be accessible at http://<JENKINS_AGENT_IP>:8090.
