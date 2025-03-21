pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('docker-hub-credentials') 
        DOCKER_IMAGE_NAME = 'joshua192/nodejsapi' 
        DOCKER_IMAGE_TAG = 'latest'
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/azumahjoshua/jenkins-cicd-demo.git', branch: 'main'
                sh 'pwd'
                sh 'ls -la'
            }
        }

        stage('Install Dependencies') {
            steps {
                dir('app') {  
                    sh 'npm install'
                }
            }
        }

        stage('Run Tests') {
            steps {
                dir('app') { 
                    script {
                        def packageJson = readJSON file: 'package.json'
                        if (packageJson.scripts.test) {
                            sh 'npm test'
                        } else {
                            echo 'No test script found in package.json. Skipping tests.'
                        }
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                dir('app') {
                    sh "docker build -t ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG} ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_HUB_USER', passwordVariable: 'DOCKER_HUB_PASSWORD')]) {
                    sh "echo ${env.DOCKER_HUB_PASSWORD} | docker login -u ${env.DOCKER_HUB_USER} --password-stdin"
                    sh "docker push ${env.DOCKER_IMAGE_NAME}:${env.DOCKER_IMAGE_TAG}"
                }
            }
        }

        stage('Deploy with Docker Compose') {
            steps {
                script {
                    // Stop and remove existing containers if they exist
                    sh 'docker-compose down || true'
                    // Start new containers
                    sh 'docker-compose up -d'
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed!'
        }
        success {
            echo 'Pipeline succeeded!'
        }
    }
}