pipeline {
    agent any

    stages {

        stage("Code") {
            steps {
                git branch: 'master',
                    url: 'https://github.com/LondheShubham153/node-todo-cicd.git'
            }
        }

        stage("Build") {
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

        stage("Test") {
            steps {
                echo "Testing the new build"
            }
        }

        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
