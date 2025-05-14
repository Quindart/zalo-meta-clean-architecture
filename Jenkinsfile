pipeline {
    agent any

    stages {
        stage('Preparation') {
            steps {
                echo 'Preparing workspace and environment...'
            }
        }
        stage('Checkout Code') {
            steps {
                git branch: 'PROD', url: 'https://github.com/Quindart/zalo-meta-clean-architecture'
            }
        }

        stage('Setup Environment Variables') {
            steps {
                configFileProvider([configFile(fileId: 'zalo-meta-clean-architecture-production', targetLocation: '.env')]) {
                    sh 'ls -la && cat .env' 
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                    docker-compose build
                    '''
                }
            }
        }

        stage('Remove Old Containers') {
            steps {
                script {
                    sh '''
                    docker-compose down
                    '''
                }
            }
        }

        stage('Deploy Application') {
            steps {
                script {
                    sh '''
                    docker-compose up -d
                    '''
                }
            }
        }
       
    }

      post {
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed!'
        }
    }
}
