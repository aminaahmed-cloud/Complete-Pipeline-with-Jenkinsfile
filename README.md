# Setting Up Complete Pipeline with Jenkinsfile

This guide will walk you through setting up a complete pipeline for building a Java Maven application, creating a Docker image, and deploying it using Jenkins with a Jenkinsfile.

## Prerequisites

- GitHub repository containing your Java Maven application.
- Docker Hub account with credentials configured in Jenkins.

## Steps

### 1. Add Jenkinsfile to Your Repository

Create a `Jenkinsfile` in the root of your GitHub repository with the following content:

```groovy
pipeline {
    agent any
    tools {
        maven 'maven-3.9'
    }
    stages {
        stage("build jar") {
            steps {
                script {
                    echo "building the application..."
                    sh 'mvn package'
                }
            }
        }

        stage("build image") {
            steps {
                script {
                    echo "building the docker image..."
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-repo', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                        sh 'docker build -t aminaaahmed323/demo-app:jma-2.0 .'
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push aminaaahmed323/demo-app:jma-2.0'
                    }
                }
            }
        }

        stage("deploy") {
            steps {
                script {
                    echo "deploying the application..."
                }
            }
        }               
    }
}
```
<img src="https://i.imgur.com/TtLyhVl.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

****

## 2. Configure Jenkins Job

- Open Jenkins dashboard.
- Create a new pipeline job or configure an existing one to use your GitHub repository.
- In the pipeline configuration, point Jenkins to your GitHub repository and select "Pipeline script from SCM".
- Choose "Git" as the SCM and provide your repository URL.
- Specify `*/main` as the branch to build.
- Save the pipeline job configuration.

## 3. Add Docker Credentials to Jenkins

- Go to Jenkins dashboard.
- Click on "Credentials" in the left sidebar.
- Select "System" and then "Global credentials".
- Click on "Add credentials" and choose "Username with password".
- Enter your Docker Hub username and password.
- Set an ID for your credentials, e.g., `docker-hub-repo`, and click "OK" to save.

## 4. Rebuild Pipeline Job

- Go back to your Jenkins pipeline job.
- Click on "Build Now" to trigger a new build.
- Jenkins will now execute the pipeline, which will build your Java Maven application, create a Docker image, and push it to Docker Hub.

<img src="https://i.imgur.com/usQI98t.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

<img src="https://i.imgur.com/Z5ccYVQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>

