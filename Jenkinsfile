pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        GCP_CREDENTIALS = credentials('your-gcp-credentials-id')
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    sh 'gcloud auth activate-service-account --key-file=${GCP_CREDENTIALS}'
                    sh 'gcloud config set project ${GCP_PROJECT}'
                    sh 'gcloud builds submit --tag gcr.io/${GCP_PROJECT}/your-app:latest .'
                }
            }
        }

        stage('Deploy to App Engine') {
            steps {
                script {
                    sh 'gcloud app deploy --version=jenkins --image-url=gcr.io/${GCP_PROJECT}/your-app:latest'
                }
            }
        }
    }
}
