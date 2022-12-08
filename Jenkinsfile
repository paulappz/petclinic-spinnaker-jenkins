
pipeline {
    agent any
    options {
        skipStagesAfterUnstable()
    }
    stages {
         stage('Clone repository') { 
            steps { 
                script{
                checkout scm
                }
            }
        }

         stage('Build Docker Image') {
            steps { 
                script{
                 app = docker.build("petclinic-spinnaker-jenkins")
                }
            }
        }
        stage('Test'){
            steps {
                 echo 'Empty'
            }
        }
         stage('Push') {
            steps {
                script{
                        docker.withRegistry('249506596551.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-2:aws-credentials') {
                    app.push("${env.BUILD_NUMBER}")
                    app.push("latest")
                    }
                }
            }
        }
    }
}
