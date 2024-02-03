pipeline {
    agent any

    environment {
        // AWS ECR URL
        REGISTRY_URL = "public.ecr.aws/r7m7o9d4"
        // Docker image base name
        IMAGE_NAME = "roberta-romi"
        // Unique tag for each build, avoiding 'latest'
        IMAGE_TAG = "0.0.${BUILD_NUMBER}"
    }

    stages {
        stage('Login to ECR') {
            steps {
                sh """
                aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${REGISTRY_URL}
                """
            }
        }

        stage('Build and Tag Image') {
            steps {
                sh """
                docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .
                docker tag ${IMAGE_NAME}:${IMAGE_TAG} ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Push to ECR') {
            steps {
                sh """
                docker push ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}
                """
            }
        }

        stage('Trigger Deploy') {
            steps {
                build job: 'RomiRobertaDeploy', wait: false, parameters: [
                    string(name: 'ROBERTA_IMAGE_URL', value: "${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}")
                ]
            }
        }
    }

    post {
        always {
            sh 'docker image prune -a --force'
        }
    }
}
