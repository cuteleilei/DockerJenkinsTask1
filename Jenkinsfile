pipeline {

    agent any

    stages {

         stage('Init') {

            steps {

                sh '''
                docker network create jenk-network || echo "Network Already Exists"
                docker stop flask-app || echo "flask-app not Running"
                docker stop nginx || echo "nginx not running"
                docker rm -f $(docker ps -qa) || true
                '''
            }

        }

        stage('Build') {

            steps {

                sh '''
                docker build -t flask-app .
                docker build -t mynginx ./nginx
                '''
            }
        }

        stage('Deploy') {

            steps {

                sh '''
                docker run -d --name flask-app --network jenk-network flask-app:latest
                docker run -d -p 80:80 --name mynginx --network jenk-network mynginx:latest
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

 