pipeline {

    agent any

    stages {
        stage('Build') {
            steps {
                sh '''
                docker build -t steoconnor/jenkins-flask:latest -t steoconnor/jenkins-flask:v${BUILD_NUMBER} .
                '''
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push steoconnor/jenkins-flask:latest
                docker push steoconnor/jenkins-flask:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl rollout restart deployment/flask-deployment
                kubectl rollout restart deployment/nginx-deployment
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
