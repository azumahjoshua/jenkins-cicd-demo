pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/azumahjoshua/jenkins-cicd-demo.git'
            }
        }

        stage('Deploy Nginx with Web App') {
            steps {
                script {
                    sh 'docker compose down'
                    sh 'docker compose up -d nginx'
                }
            }
        }
    }

    post {
        always {
            echo 'Deployment complete!'
        }
    }
}
