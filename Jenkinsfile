@Library("Shared") _
pipeline {

    agent { label 'vinod' }

    environment {
        IMAGE_NAME = "notes-app"
        IMAGE_TAG = "latest"
    }

    stages {
        stage("Hello"){
            steps{
                script{
                    hello()
                }
            }
        }
        stage('Clone Code') {
            steps {
               script{
                 clone("https://github.com/LondheShubham153/django-notes-app.git","main")
            }
        }
            
        }

        stage('Build Docker Image') {
            steps {
              script  {
                docker_build("notes-app","latest","hars30")
}
            }
        }

        stage('Push to DockerHub') {
            steps {
                script{
                    docker_push("notes-app","latest","hars30")
                
                }
            }
        }

        stage('Deploy Container') {
            steps {

             sh '''
                 sudo docker rm -f notes-app || true
                 sudo docker pull hars30/notes-app:latest
                 sudo docker run -d \
                 --name notes-app \
                 --restart always \
                 -p 8000:8000 \
                 hars30/notes-app:latest
'''
            }
        }
    }

    post {

        success {
            echo "Pipeline Executed Successfully"
        }

        failure {
            echo "Pipeline Failed"
        }

        always {

            sh '''
                docker images
                docker ps -a
            '''
        }
    }
}
