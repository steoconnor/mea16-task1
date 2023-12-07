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

                sh 'docker build -t steoconnor/jenkins-flask .'

                sh 'docker build -t steoconnor/jenkins-nginx ./nginx'

            }

        }

        stage('Push') {

            steps {

                sh '''
                docker push steoconnor/jenkins-flask
                docker push steoconnor/jenkins-nginx
                '''

            }

        }


        stage('Deploy') {

            steps {

                sh 'docker run -d --name flask-app --network jenkins-network steoconnor/jenkins-flask:latest'

                sh 'docker run -d -p 80:80 --name jenkins-nginx --network jenkins-network steoconnor/jenkins-nginx:latest'

            }

        }

         stage('Cleanup') {

            steps {

                sh 'docker system prune -f'

            }

        }

    }

    }