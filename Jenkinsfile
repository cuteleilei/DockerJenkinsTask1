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
                docker build -t cuteleilei/flask-app:latest -t cuteleilei/flask-app:v${BUILD_NUMBER} .
                docker build -t cuteleilei/mynginx -t cuteleilei/mynginx:v${BUILD_NUMBER} ./nginx
                '''
            }
        }

         stage('Push') {

            steps {

                sh '''
            
                docker push cutelelei/flask-app:latest
                docker push cuteleilei/flask-app:v${BUILD_NUMBER}
                docker push cuteleilei/mynginx:latest
                docker push cuteleilei/mynginx:v${BUILD_NUMBER}
                
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

 