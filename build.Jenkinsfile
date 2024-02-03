pipeline {
    agent any

    environment{
        ECR_URL = "public.ecr.aws/r7m7o9d4"
    }

    stages {
        stage('Build') {
            steps {
                sh '''
                aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ECR_URL
                docker build -t $ECR_URL/roberta-romi:0.0.$BUILD_NUMBER .
                docker push $ECR_URL/roberta-romi:0.0.$BUILD_NUMBER
                '''
            }
            post {
                always {
                    sh 'docker image prune -a --force'
                }
            }
        }
    }
}
