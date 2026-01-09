pipeline {
    agent any

    environment {
        SITE_ID = credentials('lemon_frontend_site_id')
        SITE_AUTH_TOKEN = credentials('auth_token')
    }

    stages {

        stage('Checkout code') {
            steps {
                git branch: 'devops-branch',
                    url: 'https://github.com/0rphx/Lemon.git'
            }
        }

        stage('Install dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh 'netlify deploy --prod --dir=build --site-id $SITE_ID'
            }
        }
    }
}
