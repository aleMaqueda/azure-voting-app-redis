pipeline {
   agent any
      stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Deploy') {
         steps {
            script {
               withKubeConfig([credentialsId: 'credentialserviceaccount1',
                                    serverUrl: 'http://192.168.0.6:8080',
                                    namespace: 'jenkins'
                                    ]) {
                  sh 'kubectl apply -f azure-vote-all-in-one-redis.yaml'
               }
            }
         }
      }
   }
}