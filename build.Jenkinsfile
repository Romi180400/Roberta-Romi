pipeline {
    agent any

    environment {
        // Define environment variables
        REGISTRY_URL = "public.ecr.aws/r7m7o9d4"
        IMAGE_NAME = "roberta-romi"
        // Use Jenkins' BUILD_NUMBER to tag the image uniquely
        IMAGE_TAG = "0.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Prepare Environment') {
            steps {
                // Prepare your environment if needed
                echo 'Preparing environment...'
            }
        }

        stage('Login to ECR') {
            steps {
                script {
                    // Use AWS CLI to login to ECR
                    // Ensure AWS credentials are configured properly
                    sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${REGISTRY_URL}"
                }
            }
        }

        stage('Build and Tag Image') {
            steps {
                script {
                    // Build the Docker image with the Jenkins build number as tag
                    sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."

                    // Tag the image for the ECR repository
                    sh "docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Push to ECR') {
            steps {
                script {
                    // Push the image to ECR
                    sh "docker push ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            // Clean up Docker images to free space
            sh 'docker image prune -f'
            // Optionally, add other cleanup steps here......
        }
    }
}

