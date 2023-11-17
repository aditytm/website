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
                git credentialsId: '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', url: 'https://github.com/aditytm/website.git'
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
