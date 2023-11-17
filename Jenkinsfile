pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        GCP_APP_ENGINE_SERVICE = 'default' // or your service name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from GitHub
                git credentialsId: 'your-github-credentials-id', url: 'https://github.com/aditytm/website.git'
            }
        }

        stage('Build') {
            steps {
                // Build your application if needed
                // For example, if it's a Maven project:
                sh 'mvn clean package'
            }
        }

        stage('Deploy to GAE') {
            steps {
                script {
                    // Deploy to Google App Engine
                    sh "gcloud app deploy --project=${GCP_PROJECT} --version=${BUILD_NUMBER} --no-promote --stop-previous-version"
                }
            }
        }
    }
}
