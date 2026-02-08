pipeline {
    agent {
        label 'ec2-node'
    }

    environment {
        DOCKER_IMAGE = "orphiic/lemon"
        DOCKER_TAG   = "${BUILD_NUMBER}"
        IMAGE_TAG   = "${BUILD_NUMBER}"
        AWS_REGION = "us-east-1"
        ECR_REPO = "802870950659.dkr.ecr.us-east-1.amazonaws.com/ore/lemon"
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
        stage('Login to ECR') {
            steps {
                sh '''
                aws ecr get-login-password --region $AWS_REGION \
                | docker login --username AWS --password-stdin $ECR_REPO
                '''
            }
        }

    

        stage('Build Image') {
            steps {
                sh 'docker build -t lemon .'
            }
        }

        stage('Tag Image') {
            steps {
                sh 'docker tag lemon:latest $ECR_REPO:$IMAGE_TAG'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $ECR_REPO:$IMAGE_TAG'
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
