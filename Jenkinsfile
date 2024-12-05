pipeline {
    agent any 
    
    stages { 
        stage('SCM Checkout') {
            steps {
                retry(3) {
                    git branch: 'main', url: 'https://github.com/Movin21/nodeapp-CI-CD.git'
                }
            }
        }
        stage('Build Docker Image') {
            steps {  
                bat 'docker build -t movinliyanage/nodeapp-cuban:%BUILD_NUMBER% .'
            }
        }
        stage('Login to Docker Hub') {
            steps {
                withCredentials([string(credentialsId: 'test-dockerhub-passw', variable: 'docker-pws')]) {
                    script {
                        bat "docker login -u movinliyanage@gmail.com -p %docker-pws%"
                    }
                }
            }
        }
        stage('Push Image') {
            steps {
                bat 'docker push movinliyanage/nodeapp-cuban:%BUILD_NUMBER%'
            }
        }
    }
    post {
        always {
            bat 'docker logout'
        }
    }
}
