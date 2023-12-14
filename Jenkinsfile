pipeline {

    agent any

    stages {
        stage('Init') {
            steps {
                script {
                    if (env.GIT_BRANCH == "origin/main") {
                    sh '''
                    kubectl create namespace prod || echo "namespace prod already exists"
                    '''
                    }
                    else if (env.GIT_BRANCH == "origin/dev") {
                    sh '''
                    kubectl create namespace dev || echo "namespace dev already exists"
                    '''
                    }
                    else {
                    sh '''
                    echo "Branch not recognised"
                    '''
                    }
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    if (env.GIT_BRANCH == "origin/main") {
                    sh '''
                    docker build -t steoconnor/jenkins-flask:latest -t steoconnor/jenkins-flask:prod-v${BUILD_NUMBER} .
                    '''
                    }
                    else if (env.GIT_BRANCH == "origin/dev") {
                    sh '''
                    docker build -t steoconnor/jenkins-flask:latest -t steoconnor/jenkins-flask:dev-v${BUILD_NUMBER} .
                    '''
                    }
                    else {
                    sh '''
                    echo "Branch not recognised"
                    '''
                    }
                }
            }
        }

        stage('Push') {
            steps {
                script {
                    if (env.GIT_BRANCH == "origin/main") {
                    sh '''
                    docker push steoconnor/jenkins-flask:latest
                    docker push steoconnor/jenkins-flask:prod-v${BUILD_NUMBER}
                    '''
                    }
                    else if (env.GIT_BRANCH == "origin/dev") {
                    sh '''
                    docker push steoconnor/jenkins-flask:latest
                    docker push steoconnor/jenkins-flask:dev-v${BUILD_NUMBER}
                    '''
                    }
                    else {
                    sh '''
                    echo "Branch not recognised"
                    '''
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    if (env.GIT_BRANCH == "origin/main") {
                    sh '''
                    kubectl apply -n prod -f ./kubernetes
                    kubectl set image deployment/flask-deployment -n prod task1=steoconnor/jenkins-flask:prod-v${BUILD_NUMBER}
                    '''
                    }
                    else if (env.GIT_BRANCH == "origin/dev") {
                    sh '''
                    kubectl apply -n dev -f ./kubernetes
                    kubectl set image deployment/flask-deployment -n dev task1=steoconnor/jenkins-flask:dev-v${BUILD_NUMBER}
                    '''
                    }
                    else {
                    sh '''
                    echo "Branch not recognised"
                    '''
                    }
                }
            }
        }

         stage('Cleanup') {
            steps {
                sh 'docker system prune -f'
            }
        }
    }
}
