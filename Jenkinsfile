pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhubusername/your-app-name"
        REGISTRY_CREDENTIAL = "docker-hub-credentials-id" // Replace with your Docker Hub credentials ID in Jenkins
        K8S_DEPLOYMENT_YAML = "k8s/deployment.yaml"
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository from GitHub
                git 'https://github.com/username/your-repository.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image
                    docker.build(DOCKER_IMAGE)
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Push the image to Docker Hub
                    docker.withRegistry('', REGISTRY_CREDENTIAL) {
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply the Kubernetes deployment YAML file to deploy the app
                    sh 'kubectl apply -f $K8S_DEPLOYMENT_YAML'
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images
            sh 'docker system prune -f'
        }
    }
}
