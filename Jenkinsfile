pipeline {
    agent any

    stages {
        stage('Verify Branch') {
            steps {
                echo "$GIT_BRANCH"
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                  cd azure-vote
                  docker image ls -a
                  docker build -t jenkins-pipeline .
                  docker image ls -a
                  cd .. '''
            }
        }
    
        stage('Start test app') {
            steps {
               sh """
                  docker-compose up -d
                  chmod 777 ./scripts/test_container.sh
                  ls -al ./scripts/test_container.sh
                  ./scripts/test_container.sh
                  """
            }
            post { 
               success {
                  echo  " ## App started successfully "
               }
               failure {
                  echo " ## App failed to start "
               }
            }
        }

        stage('Run Tests') { 
            steps {
               sh '''
                  pytest-3 ./tests/test_sample.py
                  '''
            }
        }

        stage('Stop test app') {
            steps {
               sh '''
                  docker-compose down
                  '''
            }
        }


    }
}
