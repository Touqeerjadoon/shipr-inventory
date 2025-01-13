pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'touqeerjadoon55/shipr-inventory'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'develop',
                     credentialsId: '240a9f71-d6eb-4bee-af7b-1b6e106f2d18',
                     url: 'https://github.com/Touqeerjadoon/shipr-inventory.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${env.DOCKER_IMAGE}:develop ."
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'b4666a98-9a93-47a3-aabd-0439f9dc91fb',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh """
                        echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USER} --password-stdin
                        docker push ${env.DOCKER_IMAGE}:develop
                    """
                }
            }
        }
        stage('Cleanup') {
            steps {
                sh "docker rmi ${env.DOCKER_IMAGE}:develop"
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}