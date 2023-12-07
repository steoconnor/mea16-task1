pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh 'docker rm -f $(docker ps -qa) || true'

                sh 'docker network create jenkins-network || true'

            }

        }

        stage('Build') {

            steps {

                sh 'docker build -t jenkins-flask .'

                sh 'docker build -t jenkins-nginx ./nginx'

            }

        }

        stage('Deploy') {

            steps {

                sh 'docker run -d --name flask-app --network jenkins-network jenkins-flask:latest'

                sh 'docker run -d -p 80:80 --name jenkins-nginx --network jenkins-network jenkins-nginx:latest'

            }

        }

         stage('Cleanup') {

            steps {

                sh 'docker system prune -f'

            }

        }

    }

    }