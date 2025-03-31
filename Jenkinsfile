pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "jayaprathap96/nginx-custom:latest"  // Your actual DockerHub username
        KUBECONFIG = "/root/.kube/config"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/JayaPrathap-JP/Demo-Project.git'  // Update with actual repo
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        try {
                            sh """
                                echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                                docker build -t ${DOCKER_IMAGE} -f nginx/Dockerfile .
                                docker push ${DOCKER_IMAGE}
                            """
                        } finally {
                            sh "docker logout"  // Ensure Docker logout always happens
                        }
                    }
                }
            }
        }

        stage('Deploy to KIND Kubernetes') {
            steps {
                script {
                    sh "kubectl apply -f k8s/nginx_deployment.yaml"
                }
            }
        }
    }

    post {
        success {
            echo "✅ Deployment Successful!"
        }
        failure {
            echo "❌ Deployment Failed!"
        }
    }
}
