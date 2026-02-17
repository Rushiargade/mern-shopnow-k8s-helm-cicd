pipeline {
    agent {
        label 'built-in'
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

        stage('Build Backend Image') {
            steps {
                dir('shopNow/backend') {
                    sh """
                        docker build -t $DOCKERHUB_REPO_BACKEND:${BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('Build Frontend Image') {
            steps {
                dir('shopNow/frontend') {
                    sh """
                        docker build -t $DOCKERHUB_REPO_FRONTEND:${BUILD_NUMBER} .
                    """
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh """
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push $DOCKERHUB_REPO_BACKEND:${BUILD_NUMBER}
                        docker push $DOCKERHUB_REPO_FRONTEND:${BUILD_NUMBER}
                    """
                }
            }
        }

        stage('Helm Upgrade Deployment') {
            steps {
                sh """
                    helm upgrade --install $HELM_RELEASE ./shopnow-chart \
                    -n $K8S_NAMESPACE \
                    --set backend.image.tag=${BUILD_NUMBER} \
                    --set frontend.image.tag=${BUILD_NUMBER}
                """
            }
        }
    }

    post {
        success {
            echo "Deployment Successful"
        }
        failure {
            echo "Deployment Failed"
        }
    }
}
