pipeline {
    agent any
    environment {
        IMAGE_NAME = "my-python-app"
        CONTAINER_NAME = "python-app-container"
        PORT = "5000"
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} ."
                sh "docker tag ${IMAGE_NAME}:${BUILD_NUMBER} ${IMAGE_NAME}:latest"
            }
        }
        stage('Test') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:${BUILD_NUMBER} pytest test_main.py"
            }
        }
        stage('Deploy') {
            steps {
                sh 'if [ $(docker ps -aq -f name=${CONTAINER_NAME}) ]; then docker rm -f ${CONTAINER_NAME}; fi'
                sh "docker run -d --name ${CONTAINER_NAME} -p ${PORT}:5000 ${IMAGE_NAME}:latest"
            }
        }
    }
    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed."
        }
    }
}
