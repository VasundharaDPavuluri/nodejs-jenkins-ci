pipeline {
    agent any

    environment {
        IMAGE_NAME = "vasundharadp/micro_service"
        TAG = "${BUILD_NUMBER}"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/VasundharaDPavuluri/nodejs-jenkins-ci.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    app = docker.build("${IMAGE_NAME}:${TAG}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-credentials') {
                        app.push("${TAG}")
                        app.push("latest")
                    }
                }
            }
        }

        stage('Trigger CD Pipeline') {
            steps {
                build job: 'cd-node-docker-job'
            }
        }
    }
}
