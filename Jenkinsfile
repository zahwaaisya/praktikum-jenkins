
pipeline {
    agent any

    environment {
        DOCKERHUB_REPO = 'zahwaaa/praktikum-jenkins'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code dari GitHub siap digunakan'
            }
        }

        stage('Build Docker Image') {
            steps {
                bat 'docker build -t %DOCKERHUB_REPO%:latest .'
            }
        }

        stage('Login Docker Hub') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    bat 'echo %DOCKER_PASS%| docker login -u %DOCKER_USER% --password-stdin'
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                bat 'docker push %DOCKERHUB_REPO%:latest'
            }
        }
    }

    post {
        success {
            echo 'Docker image berhasil dibuat dan dikirim ke Docker Hub!'
        }
        failure {
            echo 'Pipeline gagal. Periksa Console Output.'
        }
    }
}
