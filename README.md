# Jenkins CI/CD Pipeline with Docker

## Overview

This project demonstrates a simple CI/CD pipeline using Jenkins, GitHub, Docker, and Node.js.

The pipeline is automatically triggered when code is pushed to the GitHub repository. Jenkins then builds the Docker image, tests the Node.js application, and deploys it as a Docker container.

## Technologies Used

* GitHub – Source code repository
* Jenkins – CI/CD automation
* Docker – Application containerization
* Node.js – Application runtime
* Git – Version control

## Project Structure

```text
devops-task2-jenkins/
├── index.js
├── package.json
├── Dockerfile
├── Jenkinsfile
└── README.md
```

## Application

The project contains a simple Node.js HTTP server.

The application listens on port `3000` and returns:

```text
Hello from Jenkins CI/CD! version 2
```

## Dockerfile

The application is containerized using Docker.

The Dockerfile:

* Uses Node.js 24 Alpine as the base image.
* Sets `/app` as the working directory.
* Copies the application files into the container.
* Installs the Node.js dependencies.
* Exposes port `3000`.
* Starts the application using `node index.js`.

## Jenkins Pipeline

The Jenkins pipeline is defined in the `Jenkinsfile`.

It contains three stages:

### 1. Build

Jenkins builds the Docker image using:

```bash
docker build -t devops-task2-jenkins:latest .
```

### 2. Test

Jenkins runs the Node.js test script:

```bash
npm test
```

The test script uses:

```bash
node --check index.js
```

This checks the JavaScript syntax before deployment.

### 3. Deploy

Jenkins removes the previous application container if it exists and starts a new container:

```bash
docker stop jenkins-app || true
docker rm jenkins-app || true
docker run -d --name jenkins-app -p 3000:3000 devops-task2-jenkins:latest
```

## CI/CD Workflow

The complete workflow is:

```text
Developer
   |
   | git push
   v
GitHub Repository
   |
   | Push Webhook
   v
Jenkins
   |
   +---- Build
   |
   +---- Test
   |
   +---- Deploy
   |
   v
Docker Container
   |
   v
Node.js Application
```

## GitHub Webhook

A GitHub webhook is configured to trigger the Jenkins pipeline when code is pushed to the `main` branch.

Webhook endpoint:

```text
/github-webhook/
```

The webhook uses the `push` event and sends the event to Jenkins.

The webhook delivery was successfully verified with a green check mark.

## Jenkins Configuration

Jenkins is configured using:

* Pipeline definition: Pipeline script from SCM
* SCM: Git
* Repository: `DivyaVemulapalli/devops-task2-jenkins`
* Branch: `main`
* Jenkinsfile path: `Jenkinsfile`
* Trigger: GitHub hook trigger for GITScm polling

## Verification

The pipeline was successfully tested.

### Jenkins Pipeline

The Build, Test, and Deploy stages completed successfully.

### Docker Container

The application container was successfully deployed:

```text
jenkins-app
```

Port mapping:

```text
3000:3000
```

### Application Response

The deployed application was verified using:

```bash
curl http://localhost:3000
```

Response:

```text
Hello from Jenkins CI/CD! version 2
```

### Automatic Trigger

A test commit was pushed to GitHub.

GitHub successfully delivered the webhook, and Jenkins automatically started build `#3`, which completed successfully.

## Screenshots

The following screenshots demonstrate the project:

1. Jenkins Build and Test stages
2. Jenkins Deploy stage and successful pipeline
3. Running Docker containers
4. Application response using curl
5. Successful GitHub webhook delivery
6. Jenkins automatic build triggered by GitHub

## Key Learning Outcomes

Through this project, I learned:

* How CI/CD pipelines work using Jenkins.
* How to create a Jenkinsfile using declarative pipeline syntax.
* How to integrate Jenkins with GitHub.
* How GitHub webhooks trigger Jenkins pipelines.
* How to build Docker images from a Jenkins pipeline.
* How to run tests as part of a CI/CD pipeline.
* How to deploy a Node.js application using Docker.
* How to verify a deployed application.
* How to troubleshoot Docker permission issues in Jenkins.

## Conclusion

This project demonstrates an automated CI/CD workflow where a GitHub code push triggers Jenkins, Jenkins builds and tests the application, and the application is deployed as a Docker container.
