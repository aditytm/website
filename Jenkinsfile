pipeline {
    agent any

    environment {
        // Set environment variables if needed
        GCP_PROJECT = 'yashproject-401611'
        GCP_APP_ENGINE_SERVICE = 'default' // or your service name
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout your source code from version control
                // For example, if you're using Git:
                git 'https://github.com/aditytm/website.git/'
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
