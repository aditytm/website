pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        GCP_APP_ENGINE_SERVICE = 'default' // or your service name
        GCP_CREDENTIALS = credentials('f79769f2-6bb2-4682-922a-8ff7ac660b24') // Replace with your GCP credentials ID
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'bd6ec655-e156-4403-843c-0b6ec64aece0', url: 'https://github.com/aditytm/website.git', branch: 'main'
            }
        }

stage('Build') {
    steps {
        // Navigate to the project directory
        dir('/var/lib/jenkins/workspace/website') {
            script {
                // Use Maven tool
                def mvnHome = tool 'Maven'
                // Run Maven commands from the correct directory
                sh "cd /var/lib/jenkins/workspace/website && ${mvnHome}/bin/mvn clean package"
            }
        }
    }
}


        stage('Deploy to GAE') {
            steps {
                script {
                    // Configure Google Cloud SDK with credentials
                    withCredentials([file(credentialsId: 'f79769f2-6bb2-4682-922a-8ff7ac660b24', variable: 'GCP_KEY')]) {
                        sh "gcloud auth activate-service-account --key-file=${GCP_KEY}"
                    }

                    // Deploy to Google App Engine
                    sh "gcloud app deploy --project=${GCP_PROJECT} --version=${BUILD_NUMBER} --no-promote --stop-previous-version"
                }
            }
        }
    }
}
