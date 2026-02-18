pipeline {
    agent {
        kubernetes {
            label 'kaniko-agent'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
spec:
  serviceAccountName: jenkins
  containers:
    - name: kaniko
      image: gcr.io/kaniko-project/executor:latest
      tty: true
      volumeMounts:
        - name: docker-config
          mountPath: /kaniko/.docker

    - name: helm
      image: alpine/helm:3.12.0
      command:
        - sh
        - -c
        - "sleep 999999"
      tty: true

  volumes:
    - name: docker-config
      secret:
        secretName: dockerhub-secret
"""
        }
    }

    environment {
        DOCKERHUB_REPO_BACKEND = "rushii01/shopnow-backend"
        DOCKERHUB_REPO_FRONTEND = "rushii01/shopnow-frontend"
        HELM_RELEASE = "shopnow"
        K8S_NAMESPACE = "shopnow"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build & Push Backend') {
            steps {
                container('kaniko') {
                    sh """
                    /kaniko/executor \
                      --dockerfile=shopNow/backend/Dockerfile \
                      --context=dir://shopNow/backend \
                      --destination=$DOCKERHUB_REPO_BACKEND:${BUILD_NUMBER} \
                      --destination=$DOCKERHUB_REPO_BACKEND:latest \
                      --cleanup
                    """
                }
            }
        }

        stage('Build & Push Frontend') {
            steps {
                container('kaniko') {
                    sh """
                    /kaniko/executor \
                      --dockerfile=shopNow/frontend/Dockerfile \
                      --context=dir://shopNow/frontend \
                      --destination=$DOCKERHUB_REPO_FRONTEND:${BUILD_NUMBER} \
                      --destination=$DOCKERHUB_REPO_FRONTEND:latest \
                      --cleanup
                    """
                }
            }
        }

        stage('Helm Upgrade Deployment') {
            steps {
                container('helm') {
                    sh """
                    helm upgrade --install $HELM_RELEASE ./shopnow-chart \
                      -n $K8S_NAMESPACE \
                      --set backend.image.tag=${BUILD_NUMBER} \
                      --set frontend.image.tag=${BUILD_NUMBER}
                    """
                }
            }
        }
    }

    post {
        success {
            echo "CI/CD Pipeline Completed Successfully"
        }
        failure {
            echo "Pipeline Failed"
        }
    }
}
