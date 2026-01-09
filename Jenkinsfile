pipeline {
    agent any
    environment {
        SITE_ID = credentials('lemon_frontend_site_id')
        SITE_AUTH_TOKEN = credentials('auth_token')
    }
    stages {

        stage(Checkout code) {
            steps {

               branch 'devops-branch', url: 'https://github.com/0rphx/lemonbackend.git'

            }
        stage(install dependencies) {
            steps {
                sh 'npm run install'
            }
        stage (build)
            steps {
                sh 'npm run build'
            }    
        stage(deploy){
            steps {
                sh 'netlify deploy --prod --dir=build--site-id $SITE_ID '
            }
        }

        }
    }