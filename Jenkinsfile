pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], 
                          userRemoteConfigs: [[url: 'https://github.com/azumahjoshua/jenkins-cicd-demo.git']]
                ])
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

        stage('Test Nginx') {
            steps {
                script {
                    sh 'sleep 5'  // Give some time for Nginx to start
                    sh 'curl -I http://localhost:6060 || exit 1'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
        always {
            echo 'Pipeline execution completed.'
        }
    }
}
