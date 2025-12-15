pipeline {
    agent any

    environment {
        IMAGE_NAME = 'firastourki/firstjob1'
    }

    stages {

        /* ============================
           📥 CHECKOUT CODE
           ============================ */
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/firastourki/firstjob1.git'
            }
        }

        /* ============================
           🧱 BUILD MAVEN
           ============================ */
        stage('Build Maven') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw clean package -DskipTests'
            }
        }

        /* ============================
           🔍 SONARQUBE ANALYSIS
           ============================ */
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    withCredentials([
                        string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')
                    ]) {
                        sh '''
                            ./mvnw sonar:sonar \
                            -Dsonar.projectKey=student-management \
                            -Dsonar.projectName=student-management \
                            -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        /* ============================
           🐳 BUILD DOCKER IMAGE
           ============================ */
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:latest ."
            }
        }

        /* ============================
           🚀 DOCKER LOGIN & PUSH
           ============================ */
        stage('Docker Login & Push') {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: 'dockerhub',
                        usernameVariable: 'DOCKER_USER',
                        passwordVariable: 'DOCKER_PASS'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push ${IMAGE_NAME}:latest
                    '''
                }
            }
        }

        /* ============================
           📦 ARCHIVE JAR
           ============================ */
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    /* ============================
       🧹 POST ACTIONS
       ============================ */
    post {
        success {
            echo '✅ Pipeline terminé avec succès'
        }
        failure {
            echo '❌ Pipeline échoué'
        }
    }
}
