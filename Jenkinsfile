pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t devops-task2-jenkins:latest .'
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