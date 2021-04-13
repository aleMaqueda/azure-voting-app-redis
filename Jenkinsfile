pipeline {
   agent any
   environment {
      PROJECT_ID = 'proyecto2021-310522'
      CLUSTER_NAME = 'autopilot-cluster-1'
      LOCATION = 'us-central1'
      CREDENTIALS_ID = 'proyecto2021'
   }
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
      stage('Push container'){
         steps{
            echo "Workspace is $WORKSPACE"
            dir("$WORKSPACE/azure-vote"){
               script {
                  docker.withRegistry('https://index.docker.io/v1/', 'DockerHub') {
                     def image = docker.build('maqueda/jenkins-course:latest')
                     image.push()
                  }
               }
            }   
         }
      }
      // stage('Container scanning'){
      //    parallel{
      //       stage('Run Trivy'){
      //          steps{
      //             sleep(time: 30, unit: 'SECONDS')
      //             /*
      //             sh '''
      //             trivy maqueda/jenkins-course
      //             '''
      //             */
      //          }
      //       }
      //       stage('Run Anchore'){
      //          steps{
      //             sh '''
      //             echo  "maqueda/jenkins-course" > anchore_images
      //             '''
      //             anchore bailOnFail: false, bailOnPluginFail: false, name: 'anchore_images'
      //          }
      //       }
      //    }
      // }
      stage('Approve GKE deploy'){
         when {
            branch 'master'
         }
         options{
            timeout(time: 1, unit: 'MINUTES')
         }
         steps{
            input message: "Deploy?"
         }
         post {
            success {
               echo 'Deploy approved'
            }
            aborted {
               echo 'Deploy denied'
            }
         }
      }
      stage('Deploy GKE'){
         when {
            branch 'master'
         }
         steps{
            step([
               $class: 'KubernetesEngineBuilder',
               projectId: env.PROJECT_ID,
               clusterName: env.CLUSTER_NAME,
               location: env.LOCATION,
               manifestPattern: 'azure-vote-all-in-one-redis.yaml',
               credentialsId: env.CREDENTIALS_ID,
               verifyDeployments: true])
         }
      }
   }
}
