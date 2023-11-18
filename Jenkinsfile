pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
         CLIENT_EMAIL='adityagurjar20001234123@yashproject-401611.iam.gserviceaccount.com'
        GCP_CREDENTIALS_ID = '8d5ba43d-6f56-4012-ac4c-a18717458832'
    }

    stages {
        stage('Authenticate with GCP') {
            steps {
                script {
                    withCredentials([file(credentialsId: GCP_CREDENTIALS_ID, variable: 'GCP_KEY')]) {
                        // Authenticate with GCP using the service account JSON key
                        sh """
                            gcloud auth activate-service-account --key-file=${GCP_KEY}
                            gcloud config set project ${GCP_PROJECT}
                        """
                    }
                }
            }
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

    }
}
