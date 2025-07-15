pipeline {
  agent {
    kubernetes {
      inheritFrom 'kaniko'
    }
  }
  environment {
    IMAGE = 'jayaprathap96/nginx-custom:latest'
  }
  stages {
    stage('Kaniko Build') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor --dockerfile=Dockerfile --context=. --destination=$IMAGE
          '''
        }
      }
    }

    stage('Deploy to KIND Kubernetes') {
      steps {
        container('kubectl') { // Assuming you have kubectl container in pod template
          sh 'kubectl apply -f k8s/nginx_deployment.yaml'
        }
      }
    }
  }
}
