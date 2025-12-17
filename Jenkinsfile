@Library("Shared") _
pipeline {
    agent { label "agent2" }

    stages {

        stage("Code") {
            steps {
               script{
                clone("https://github.com/gopaljha16/node-todo-cicd.git" , "main")
               }
            }
        }

        stage("Trivy File System Scan"){
            steps{
                script{
                    trivy_fs()
                }
            }
        }

        stage("Build and Test") {
            steps {
                sh 'docker build -t gopal161/node-todo-test:latest .'
            }
        }

        stage("Push") {
            steps {
                withCredentials([
                    usernamePassword(
                        credentialsId: "dockerHub",
                        usernameVariable: "dockerHubUser",
                        passwordVariable: "dockerHubPassword"
                    )
                ]) {
                    sh '''
                        echo $dockerHubPassword | docker login -u $dockerHubUser --password-stdin
                        docker push gopal161/node-todo-test:latest
                        docker logout
                    '''
                }
            }
        }

        stage("Deploy") {
            steps {
                // sh "docker-compose down && docker-compose up -d"
                script{
                    docker_deploy()
                }
            }
        }
    }

 post {
        success {
            script {
                email_notification(
                    "gopaljha9398@gmail.com",
                    "gopaljha9398715741@gmail.com",
                    "SUCCESS"
                )
            }
        }

        failure {
            script {
                email_notification(
                    "gopaljha9398@gmail.com",
                    "gopaljha9398715741@gmail.com",
                    "FAILURE"
                )
            }
        }
    }
}
