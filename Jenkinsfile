pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        GCP_APP_ENGINE_SERVICE = 'default' // or your service name
        CLIENT_EMAIL = '143055914841-compute@developer.gserviceaccount.com'
        GCP_CREDENTIALS = credentials('1ee2a44f-3263-41f5-8225-364e7f1c95e1') // Replace with your GCP credentials ID
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
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

        stage('Gcloud Compute') {
            steps {
                script {
                    sh 'gcloud compute zones list'
                }
            }
        }

        stage('Deploy to GAE') {
            steps {
                script {
                    // Configure Google Cloud SDK with credentials
                    withGCP_CREDENTIALS([file(credentials: 'your-gcp-credentials-id', variable: 'GCP_KEY')]) {
                        sh "gcloud auth activate-service-account --key-file=${GCP_KEY}"

                        // Deploy to Google App Engine
                        sh "gcloud app deploy --project=${GCP_PROJECT} --version=${BUILD_NUMBER} --no-promote --stop-previous-version"
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                sh 'gcloud auth revoke $CLIENT_EMAIL'
            }
        }
    }
}
