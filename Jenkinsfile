pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh '''
                ssh -i ~/.ssh/id_rsa jenkins@10.154.0.27 << EOF
                docker rm -f $(docker ps -qa) || true
                docker network create jenkins-network || true
                docker stop flask-app || echo "flask-app Not Running"
                docker stop jenkins-nginx || echo "nginx Not Running"
                docker rm flask-app || echo "flask-app Not Running"
                docker rm jenkins-nginx || echo "nginx Not Running"
                '''

            }

        }

        stage('Build') {

            steps {

                sh '''
                docker build -t steoconnor/jenkins-flask:latest -t steoconnor/jenkins-flask:v${BUILD_NUMBER} .
                docker build -t steoconnor/jenkins-nginx -t steoconnor/jenkins-nginx:v${BUILD_NUMBER} ./nginx
                '''
            }

        }

        stage('Push') {

            steps {

                sh '''
                docker push steoconnor/jenkins-flask:latest
                docker push steoconnor/jenkins-nginx:latest
                docker push steoconnor/jenkins-flask:v${BUILD_NUMBER}
                docker push steoconnor/jenkins-nginx:v${BUILD_NUMBER}
                '''

            }

        }


        stage('Deploy') {

            steps {

                sh '''
                docker run -d --name flask-app --network jenkins-network steoconnor/jenkins-flask:latest
                docker run -d -p 80:80 --name jenkins-nginx --network jenkins-network steoconnor/jenkins-nginx:latest
                '''

            }

        }

         stage('Cleanup') {

            steps {

                sh 'docker system prune -f'

            }

        }

    }

    }