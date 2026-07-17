pipeline {

    agent any

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/ahamedyasiksarvaththudeen/simple-node-app.git'
            }
        }

        stage('Verify Project Files') {
            steps {
                bat 'dir'
            }
        }

        stage('Check Node Version') {
            steps {
                bat 'node -v'
                bat 'npm -v'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test'
            }
        }

        stage('Check Docker') {
            steps {
                bat 'docker --version'
                bat 'docker info'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t simple-node-app:latest .'
            }
        }

        stage('List Docker Images') {
            steps {
                bat 'docker images'
            }
        }

        stage('Stop Existing Container') {
            steps {
                bat '''
                docker stop simple-node-container >nul 2>&1
                exit /b 0
                '''
            }
        }

        stage('Remove Existing Container') {
            steps {
                bat '''
                docker rm simple-node-container >nul 2>&1
                exit /b 0
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker run -d ^
                --name simple-node-container ^
                -p 9091:3000 ^
                simple-node-app:latest
                '''
            }
        }

        stage('Running Containers') {
            steps {
                bat 'docker ps'
            }
        }

        stage('Application Logs') {
            steps {
                bat 'docker logs simple-node-container'
            }
        }

    }

    post {

        always {
            echo 'Pipeline Completed'
        }

        success {
            echo 'Build Successful'
            echo 'Application is available at http://localhost:9091commi'
        }

        failure {
            echo 'Build Failed'
        }
    }
}