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

        stage('Push Container') {
            steps {
               echo " Workspace is $WORKSPACE"
               dir("$WORKSPACE/azure-vote") {
                  script {
                     docker.withRegistry('https://index.docker.io.v1/', 'DockerHub')
                        def image = docker.build('c001/jenkins-course:latest')
                        image.psuh()
                  }
               }
            }
        }



    }
}
