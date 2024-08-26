pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'python-app'  // Docker image name
        KUBE_CONFIG = '/root/.kube/config'  // Path to Kubernetes config
    }

    stages {
        stage('Clone') {
            steps {
                // Clone the Git repository
                git 'https://github.com/Jofralso/test_CICD'
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the Docker image
                    docker.build("${DOCKER_IMAGE}")
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    // Run tests in a Docker container
                    docker.image("${DOCKER_IMAGE}").inside {
                        sh 'pytest'
                    }
                }
            }
        }

        stage('Deploy to Minikube') {
            steps {
                script {
                    // Configure kubectl to use Minikube
                    sh 'kubectl config use-context minikube'
                    
                    // Apply Kubernetes deployment configuration
                    sh 'kubectl apply -f deployment.yaml'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            sh 'docker image prune -f'
        }
    }
}
