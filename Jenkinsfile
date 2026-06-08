pipeline {

    agent any

    tools {
        jdk 'JDK17'
        maven 'Maven3'
    }

    environment {
        IMAGE_NAME = "shipment-service"
        IMAGE_TAG = "${BUILD_NUMBER}"
        DOCKER_REPO = "shobith031"
    }


    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }


        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }


        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }


        stage('SonarQube') {
            steps {
                sh 'mvn sonar:sonar -Dsonar.projectKey=shipment-service'
            }
        }


        stage('Docker Build') {
            steps {
                sh '''
                docker build \
                -t ${IMAGE_NAME}:${IMAGE_TAG} .
                '''
            }
        }


        stage('Trivy Scan') {
            steps {
                sh '''
                trivy image ${IMAGE_NAME}:${IMAGE_TAG}
                '''
            }
        }


        stage('Deploy') {
            steps {
                sh '''
                docker compose down
                docker compose up -d --build
                '''
            }
        }
    }
}
