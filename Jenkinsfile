pipeline {
    agent any
       triggers {
        pollSCM "* * * * *"
       }
    stages {
        stage('Build Application') { 
            steps {
                echo '=== Building Petclinic Application ==='
                sh 'mvn -B -DskipTests clean package' 
            }
        }
        // stage('Test Application') {
        //     steps {
        //         echo '=== Testing Petclinic Application ==='
        //         sh 'mvn test'
        //     }
        //     post {
        //         always {
        //             junit 'target/surefire-reports/*.xml'
        //         }
        //     }
        // }
        // stage('Build Docker Image') {
        //     when {
        //         branch 'dev'
        //     }
        //     steps {
        //         echo '=== Building Petclinic Docker Image ==='
        //         script {
        //             app = docker.build("paulappz/petclinic-spinnaker-jenkins")
        //         }
        //     }
        // }
        // stage('Push Docker Image') {
        //     when {
        //         branch 'dev'
        //     }
        //     steps {
        //         echo '=== Pushing Petclinic Docker Image ==='
        //         script {
        //             GIT_COMMIT_HASH = sh (script: "git log -n 1 --pretty=format:'%H'", returnStdout: true)
        //             SHORT_COMMIT = "${GIT_COMMIT_HASH[0..7]}"
        //             docker.withRegistry('https://registry.hub.docker.com', 'dockerHubCredentials') {
        //                 app.push("$SHORT_COMMIT")
        //                 app.push("latest")
        //             }
        //         }
        //     }
        // }

    stage('Build Docker Image') {
            when {
                branch 'dev'
            }
            steps { 
                script{
                 app = docker.build("petclinic-spinnaker-jenkins")
                }
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
