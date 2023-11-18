pipeline {
    agent any

    environment {
        GOOGLE_CLOUD_PROJECT = 'yashproject-401611'
        APP_ENGINE_SERVICE = 'default'  // Change to your App Engine service name
        CLOUDSDK_CORE_DISABLE_PROMPTS = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Checkout the source code from GitHub
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // You can add build steps here if needed
                    // For example: ./gradlew build
                }
            }
        }

        stage('Deploy to App Engine') {
            steps {
                script {
                    // Authenticate with Google Cloud
                    sh "gcloud auth activate-service-account --key-file=/home/adityagurjar20001234123/.gcp/key.json"

                    // Set Google Cloud project
                    sh "gcloud config set project ${GOOGLE_CLOUD_PROJECT}"

                    // Deploy to App Engine
                    sh "gcloud app deploy --version=${BUILD_NUMBER} --quiet app.yaml"
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment successful!'
        }

        failure {
            echo 'Deployment failed!'
        }
    }
}
