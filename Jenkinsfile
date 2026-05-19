pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat '"%PYTHON%" -m venv venv'
                bat 'venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                bat 'venv\\Scripts\\pytest'
            }
        }

        stage('Deploy') {
            steps {
                bat 'start /B venv\\Scripts\\python app.py'
            }
        }
    }
}