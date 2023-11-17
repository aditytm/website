pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        GCP_APP_ENGINE_SERVICE = 'default' // or your service name
        GCP_CREDENTIALS = credentials('your-gcp-credentials-id') // Replace with your GCP credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'bd6ec655-e156-4403-843c-0b6ec64aece0', url: 'https://github.com/aditytm/website.git', branch: 'main'
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
                    // Configure Google Cloud SDK with credentials
                    withCredentials([file(credentialsId: 'your-gcp-credentials-id', variable: 'GCP_KEY')]) {
                        sh "gcloud auth activate-service-account --key-file=${GCP_KEY}"
                    }

                    // Deploy to Google App Engine
                    sh "gcloud app deploy --project=${GCP_PROJECT} --version=${BUILD_NUMBER} --no-promote --stop-previous-version"
                }
            }
        }
    }
}
