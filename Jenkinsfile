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
    stage('Checkout Code') {
      steps {
        git branch: 'master', url: 'https://github.com/JayaPrathap-JP/Demo-Project.git'
      }
    }

    stage('Kaniko Build & Push') {
      steps {
        container('kaniko') {
          sh '''
            /kaniko/executor --dockerfile=/nginx/Dockerfile --context=/home/jenkins/agent/workspace/demo --destination=$IMAGE
          '''
        }
      }
    }

    stage('Deploy to KIND Kubernetes') {
      steps {
        container('kubectl') {
          sh 'kubectl apply -f deploy.yaml || true'
          sh 'kubectl rollout status deployment/nginx-deployment --timeout=60s || true'
        }
      }
    }

    stage('Post Info') {
      steps {
        echo "✅ Image built and pushed: $IMAGE"
      }
    }
  }
}
