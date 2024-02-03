pipeline {
    agent any

   stages {
        stage('Build') {
            steps {
                withCredentials([string(credentialsId: 'Roberta', variable: 'TOKEN')]) {
                    sh 'echo building...'
            }
        }
    }
}
