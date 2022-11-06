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
                  cd jenkins/build/ 
                  sudo docker-compose up -d
                '''
            }
        }
        stage('Test') {
           steps {
               sh '''
                  docker-compose exec -T app python tests/test.py
               '''
        }
        }
        
        stage('Remove Test container') {
           steps {
               sh '''
                  docker-compose down
               '''
        }
        }
//         stage('Deploy') {
//             steps {
//                 sh '''
//                  cd jenkins/build
//                  export $BUILD_NUMBER
//                  sudo docker-compose up -d
                 
//                 '''
//             }
//         }
    }
}
