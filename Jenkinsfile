pipeline {
    agent any

    environment {
        GCP_PROJECT = 'yashproject-401611'
         CLIENT_EMAIL='adityagurjar20001234123@yashproject-401611.iam.gserviceaccount.com'
        GCP_CREDENTIALS_ID = '8d5ba43d-6f56-4012-ac4c-a18717458832'
    }

 stages {
    stage('Gcloud Auth') {
      steps {
        sh '''
          gcloud version
          gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
          echo "my project id is $CLOUDSDK_CORE_PROJECT"
          gcloud compute zones list
        '''
      }
    }

     stage('Gcloud Compute') {
      steps {
        sh '''
          gcloud compute zones list
        '''
      }
    }

     stage('Gcloud App Engine') {
      steps {
        sh '''
          Zero_Traffic=$(gcloud app versions list --filter="traffic_split=0" --format="table(version.id)" --project $CLOUDSDK_CORE_PROJECT | tail -n +2)
          export Zero_Traffic
          for row in $Zero_Traffic
            do
              echo  $row
              yes | gcloud app versions delete $row --project $CLOUDSDK_CORE_PROJECT
            done
        '''
      }
    }




  }


 post {
    always {
      sh 'gcloud auth revoke $CLIENT_EMAIL'
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
