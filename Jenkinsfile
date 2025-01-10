pipeline {
    agent any
    environment {
        DOCKER_IMAGE = 'touqeerjadoon55/shipr-inventory'
    }
    parameters {
        gitParameter name: 'BRANCH',
                     type: 'PT_BRANCH',
                     defaultValue: 'develop',
                     description: 'Select the branch to build and deploy'
    }
    stages {
        stage('Checkout') {
            steps {
                git(
                    url: 'https://github.com/Touqeerjadoon/shipr-inventory.git',
                    credentialsId: '240a9f71-d6eb-4bee-af7b-1b6e106f2d18',
                    branch: "${params.BRANCH}"
                )
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE}:${params.BRANCH} .'
            }
        }
        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'b4666a98-9a93-47a3-aabd-0439f9dc91fb',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASSWORD'
                )]) {
                    sh 'echo ${DOCKER_PASSWORD} | docker login -u ${DOCKER_USER} --password-stdin'
                    sh 'docker push ${DOCKER_IMAGE}:${params.BRANCH}'
                }
            }
        }
    }
}