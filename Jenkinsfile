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
        S3_BUCKET  = "lemon-buckett"
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
       
        stage('Deploy to S3') {
            steps {
                withCredentials([
                    [$class: 'AmazonWebServicesCredentialsBinding',
                     credentialsId: 'awscreds']
                ]) {
                    sh '''
                      aws s3 sync build/ s3://$S3_BUCKET --delete --region $AWS_REGION
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
            echo 'Pipeline failed 1❌'
        }
    }
}
