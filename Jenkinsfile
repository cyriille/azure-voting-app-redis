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
    }
}
