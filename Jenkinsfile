pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
        CLIENT_EMAIL = 'adityagurjar20001234123@gmail.com'
        GCP_CREDENTIALS_ID = 'e1617e72-fd8d-4c0e-b916-ae8fcfa9cb65'
    }

    stages {
        stage('Gcloud Auth') {
            steps {
                script {
                    sh """
                        gcloud version
                        gcloud auth activate-service-account --key-file="${HOME}/.gcp/key.json"
                        echo "my project id is \${CLOUDSDK_CORE_PROJECT}"
                        gcloud compute zones list
                    """
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

        stage('Gcloud App Engine') {
            steps {
                script {
                    def Zero_Traffic = sh(
                        script: 'gcloud app versions list --filter="traffic_split=0" --format="table(version.id)" --project $CLOUDSDK_CORE_PROJECT | tail -n +2',
                        returnStatus: true
                    ).trim()

                    sh "export Zero_Traffic=${Zero_Traffic}"
                    sh """
                        for row in \${Zero_Traffic}
                        do
                            echo \$row
                            yes | gcloud app versions delete \$row --project \$CLOUDSDK_CORE_PROJECT
                        done
                    """
                }
            }
        }

        stage('Checkout') {
            steps {
                script {
                    git credentialsId: 'bd6ec655-e156-4403-843c-0b6ec64aece0', url: 'https://github.com/aditytm/website.git', branch: 'main'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Navigate to the project directory
                    dir('/var/lib/jenkins/workspace/website') {
                        // Use Maven tool
                        def mvnHome = tool 'Maven'
                        // Run Maven commands from the correct directory
                        sh "cd /var/lib/jenkins/workspace/website && ${mvnHome}/bin/mvn clean package"
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
