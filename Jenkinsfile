pipeline {
    agent any
    stages {

        stage('Build') {
            steps {
                sh '''
                docker build -t cuteleilei/flask-app:latest -t cuteleilei/flask-app:v${BUILD_NUMBER} .
                '''
            }
        }

        stage('Push') {
            steps {
                sh '''
                docker push cuteleilei/flask-app:latest
                docker push cuteleilei/flask-app:v${BUILD_NUMBER}
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                kubectl apply -f ./kubernetes
                kubectl set image deployment/flask-deployment task1=cuteleilei/flask-app:v${BUILD_NUMBER}
                '''
            }
        }

        stage('CleanUp') {
            steps {
                sh '''
                docker system prune -f 
                '''
            }
        }
    }

}