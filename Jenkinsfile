pipeline {
    agent any  // Use master (your Windows Jenkins)

    environment {
        DOCKER_IMAGE = "jayaprathap96/nginx-custom:latest"
        KUBECONFIG = "C:\\Users\\JAYAPRATHAP-JP\\.kube\\config" // Change this to your actual path on Windows
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master', url: 'https://github.com/JayaPrathap-JP/Demo-Project.git'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                        try {
                            bat """
                                echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
                                docker build -t ${DOCKER_IMAGE} -f nginx/Dockerfile .
                                docker push ${DOCKER_IMAGE}
                            """
                        } finally {
                            bat "docker logout"
                        }
                    }
                }
            }
        }

        stage('Deploy to KIND Kubernetes') {
            steps {
                script {
                    bat "kubectl --kubeconfig=${KUBECONFIG} apply -f k8s/nginx_deployment.yaml"
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
