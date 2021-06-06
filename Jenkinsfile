pipeline {
    agent any
    stages {
        stage('Test') {
            steps {
                echo 'run nginx pod'
                script {
                  withKubeConfig([credentialsId: 'credentialserviceaccount1',
                                          serverUrl: 'http://192.168.0.6:8080',
                                          namespace: 'jenkins'
                                          ]) {
                           sh 'kubectl run nginxtest --image=nginx'
                        }
                  }
            }
        }
    }
}