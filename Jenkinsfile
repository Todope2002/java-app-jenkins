pipeline {
    agent any

    tools{
        maven 'maven3'
        jdk  'jdk17'
    }

    stages {
        stage('Checkout Code') {
            step {
                checkout scm
            }
        }

        stage('Build Java Application') {
            steps {
                sh 'mvn clean package DskipTests'
            }
        }   
        
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t dominiondpt1/my-java-app:v1 .'
            }
        }

        stage('Push docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'DOCKER_LOGIN',
                    usernameVariable: 'USERNAME',
                    passwordVariable: 'PASSWORD'
                )]) {
                    sh '''
                        echo $PASSWORD | docker login -u $USERNAME --password-stdin
                        docker push dominiondpt1/my-java-app:v1
                        docker logout
                        '''    
                }
            }
        }
    }

    post {
        success {
            echo '✅ Build and Docker image-push successful'
        }
        failure {
            echo '❌ Build Failed. Check the logs for details'
        }
    }
    
}