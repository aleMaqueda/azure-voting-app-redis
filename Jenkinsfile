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
            sh '''
            cd azure-vote/; 
            docker build -t jenkins-pipeline .
            docker images -a | grep jenkins-pipeline
            cd ..
            '''
         }
      }
   }
}
