pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/firastourki/firstjob.git'
            }
        }

        stage('Build') {
            steps {
                dir('student-management') {
                    sh './mvnw clean install'
                }
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
            }
        }
    }
}
