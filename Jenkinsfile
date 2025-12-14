pipeline {
    agent any

    environment {
        IMAGE_NAME = 'firastourki/firstjob1'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/firastourki/firstjob1.git'
            }
        }

        stage('Build Maven') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([
                    string(credentialsId: 'dockerhub-token', variable: 'DOCKERHUB_TOKEN')
                ]) {
                    sh '''
                        echo "$DOCKERHUB_TOKEN" | docker login -u firastourki --password-stdin
                        docker push firastourki/firstjob1:latest
                    '''
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
            }
        }
    }
}

