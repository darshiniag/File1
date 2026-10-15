  pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "shreyavinay30/shreya"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/darshiniag/File1.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKER_IMAGE%:v1.'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(
                    credentialsId: 'dockerhub-PAT',
                    variable: 'DOCKER_USER'
                )]) {
                    bat '''
                    @echo off
                    echo %DOCKER_PAT%|docker login -u darshiniag --password-stdin
                   if errorlevel 1 exit /b 1
                    '''
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKER_IMAGE%:'
            }
        }
    }

    post {
        success {
            echo 'Docker image built and pushed successfully.'
        }
        failure {
            echo 'Pipeline failed. Check Console Output.'
        }
    }
}
