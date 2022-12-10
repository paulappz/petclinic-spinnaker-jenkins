
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
            stage('Test Application') {
        steps {
            echo '=== Testing Petclinic Application ==='
            // sh 'mvn test'
        }
        // post {
        //     always {
        //         junit 'target/surefire-reports/*.xml'
        //     }
        // }
    }
         stage('Push') {
            steps {
                script{
                    GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
                    SHORT_COMMIT = "${GIT_COMMIT_HASH[0..7]}"
                    docker.withRegistry('https://249506596551.dkr.ecr.us-east-1.amazonaws.com', 'ecr:us-east-1:aws-credentials') {
                        app.push("$SHORT_COMMIT")
                        app.push("latest")
                    }
                }
            }
        }
        stage('Remove local images') {
            steps {
                echo '=== Delete the local docker images ==='
                
                sh("docker rmi -f 249506596551.dkr.ecr.us-east-1.amazonaws.com/petclinic-spinnaker-jenkins:$SHORT_COMMIT || :")
                sh("docker rmi -f 249506596551.dkr.ecr.us-east-1.amazonaws.com/petclinic-spinnaker-jenkins:latest || :")
                sh("docker image ls")
            }
        }
    }
}
