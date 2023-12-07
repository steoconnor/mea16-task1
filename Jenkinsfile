pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh '''
                docker rm -f $(docker ps -qa) || true
                docker stop flask-app || echo "flask-app not running"
                docker stop nginx || echo "nginx not running"
                docker network create jenk-network || echo "Network already exists"
                '''
            }

        }

        stage('Build') {

            steps {

                sh '''
                docker build -t flask-jenkins .
                docker build -t nginx-jenkins -f ./nginx
                '''
            }

        }

        stage('Deploy') {

            steps {

                sh 'docker run -d --name flask-app --network jenk-network flask-jenkins:latest'

                sh 'docker run -d -p 80:80 --name nginx --network jenk-network nginx-jenkins:latest'

            }

        }

    }

}