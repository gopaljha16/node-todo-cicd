pipeline {
    agent { label "node-agent"}

    stages {

        stage("Code") {
            steps {
                git branch: 'main',
                    url: 'https://github.com/gopaljha16/node-todo-cicd.git'
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
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
