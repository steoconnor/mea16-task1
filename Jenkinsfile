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
                kubectl set image deployment/flask-deployment task1=steoconnor/flask-jenk:v${BUILD_NUMBER}
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
