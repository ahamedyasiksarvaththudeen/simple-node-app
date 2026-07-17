pipeline {

    agent any

    environment {
        REPO_URL = 'https://github.com/ahamedyasiksarvaththudeen/simple-node-app.git'
        BRANCH = 'main'

        IMAGE_NAME = 'simple-node-app:latest'
        CONTAINER_NAME = 'simple-node-container'

        HOST_PORT = '8084'
        CONTAINER_PORT = '3000'
    }

    stages {

        stage('Clean Workspace') {
            steps {
                deleteDir()
            }
        }

        stage('Clone Repository') {
            steps {
                git branch: "${BRANCH}",
                    url: "${REPO_URL}"
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
                bat '''
                docker build -t %IMAGE_NAME% .
                '''
            }
        }

        stage('List Docker Images') {
            steps {
                bat '''
                docker images
                '''
            }
        }

        stage('Stop Existing Container') {
            steps {
                bat '''
                docker stop %CONTAINER_NAME% >nul 2>&1
                exit /b 0
                '''
            }
        }

        stage('Remove Existing Container') {
            steps {
                bat '''
                docker rm %CONTAINER_NAME% >nul 2>&1
                exit /b 0
                '''
            }
        }

        stage('Run Docker Container') {
            steps {
                bat '''
                docker run -d ^
                --name %CONTAINER_NAME% ^
                -p %HOST_PORT%:%CONTAINER_PORT% ^
                %IMAGE_NAME%
                '''
            }
        }

        stage('Running Containers') {
            steps {
                bat '''
                docker ps
                '''
            }
        }

        stage('Application Logs') {
            steps {
                bat '''
                docker logs %CONTAINER_NAME%
                '''
            }
        }

    }

    post {

        always {
            echo '====================================='
            echo 'Pipeline Completed'
            echo '====================================='
        }

        success {
            echo 'Build Successful'
            echo 'Application URL: http://localhost:9090'
        }

        failure {
            echo 'Build Failed'
        }
    }
}