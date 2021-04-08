pipeline {
   agent any

   stages {
      stage('Verify Branch') {
         steps {
            echo "$GIT_BRANCH"
         }
      }
      stage('Docker Build') {
         steps {
            sh '''
            cd azure-vote/; 
            docker build -t jenkins-pipeline .
            docker images -a | grep jenkins-pipeline
            cd ..
            '''
         }
      }
      stage('Start test app') {
         steps {
            sh '''
               docker-compose up -d
              
            '''
         }
         post {
            success {
               echo "App started successfully :)"
            }
            failure {
               echo "App failed to start :("
            }
         }
      }
      stage('Run Tests') {
         steps {
            sh '''
               python ./tests/test_sample.py
            '''
         }
      }
      stage('Stop test app') {
         steps {
            sh '''
               docker-compose down
            '''
         }
      }
      stage{
         steps{
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote"){
               script {
                  docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                     def image = docker.build('blackdentech/jenkins-course:latest')
                     image.push()
                  }
               }
            }   
         }
      }
   }
}
