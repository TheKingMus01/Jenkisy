pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo "Pulling code from GitHub..."
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo "Building the app..."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
            }
        }
    }
}