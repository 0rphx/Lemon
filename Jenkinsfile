pipeline {
    agent {
        label 'ec2-node'
    }

    environment {
        DOCKER_IMAGE = "orphiic/lemon"
        DOCKER_TAG   = "latest"
    }

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/0rphx/Lemon.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                  docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                '''
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([string(credentialsId: 'dockerhub-password', variable: 'DOCKER_PASSWORD')]) {
                    sh '''
                      echo "$DOCKER_PASSWORD" | docker login -u orphiic --password-stdin
                      docker push $DOCKER_IMAGE:$DOCKER_TAG
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Build and Dockerization successful 🎉'
        }
        failure {
            echo 'Pipeline failed ❌'
        }
    }
}
