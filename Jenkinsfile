pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        GCP_APP_ENGINE_SERVICE = 'default' // or your service name
        GCP_CREDENTIALS = credentials('6a745940-cbb8-44db-8bce-7c95ada646d3') // Replace with your GCP credentials ID
        GOOGLE_APPLICATION_CREDENTIALS = credentials('6a745940-cbb8-44db-8bce-7c95ada646d3')
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

        stage('Authenticate with GCP') {
            steps {
                script {
                    sh 'gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS'
                }
            }
        }

        // Additional stages...
    }
}
environment {
    GOOGLE_APPLICATION_CREDENTIALS = credentials('6a745940-cbb8-44db-8bce-7c95ada646d3')
}
