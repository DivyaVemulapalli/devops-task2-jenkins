pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'docker run --rm -v "$PWD":/app -w /app node:24-alpine npm test'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker stop jenkins-app || true'
                sh 'docker rm jenkins-app || true'
                sh 'docker run -d --name jenkins-app -p 3000:3000 devops-task2-jenkins:latest'
            }
        }
    }
}