pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker build'){
         steps{
            sh('docker ps -a')
            sh('''
            cd azure-vote/; 
            docker build -t jenkins-pipeline
            docker images -a
            cd ..
            ''')
         }
      }
   }
}
