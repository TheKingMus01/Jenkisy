pipeline {
    agent any

    environment {
        IMAGE_NAME = "thekingmus/jenkins-demo"
        KUBE_NAMESPACE = "default"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub',
                                                 usernameVariable: 'USER',
                                                 passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_NAME}:latest"
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withCredentials([file(credentialsId: 'kubeconfig-ci', variable: 'KUBECONFIG')]) {
                sh """
                    kubectl set image deployment/jenkins-demo \
                    jenkins-demo=${IMAGE_NAME}:latest \
                    -n ${KUBE_NAMESPACE}
                """
                 // Force pods to restart if needed
                sh "kubectl rollout status deployment/jenkins-demo -n ${KUBE_NAMESPACE}"
                sh "kubectl rollout restart deployment/jenkins-demo -n ${KUBE_NAMESPACE}"
                }
            }
        }
    }
    
    
    post {
        always {
            echo "Pipeline finished."
        }
    }
}