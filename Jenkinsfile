pipeline {

    agent any

    stages {

        stage('Build and test image') {
            steps {
                sh '''
                
                  cp -r static templates tests jenkins/build/
                  cp .env hello.py requirements.txt jenkins/build/
                  echo "****************************"
                  echo "** Building Docker Image ***"
                  echo "****************************"
                  cd jenkins/build/ && docker-compose build && docker-compose up -d 
                '''
            }
        }
        stage('Test') {
           steps {
               sh '''
                 cd jenkins/build/ && docker-compose exec -T app python tests/test.py
               '''
        }
        }
        
        stage('Remove Test container') {
           steps {
               sh '''
                  cd jenkins/build/ && docker-compose down
               '''
        }
        }
        stage('Deploy in Development') {
            steps {
                sh '''
                 cd jenkins/build && sudo sed -E -i'' "s/(.*python:).*/\1$BUILD_NUMBER/" 'docker-compose.yml' && sudo docker-compose up -d   
                '''
            }
        }
    }
}
