pipeline {
    environment {
      DOCKER = credentials('dockerhub')
    }
  agent any
  stages {
// Image build
    stage('BUILD') {
      parallel {
        stage('Web image') {
          steps {
            sh 'docker login -u=$DOCKER_USR -p=$DOCKER_PSW'
            sh 'docker build -f web/Dockerfile -t mariogelo/web:latest web/.'
          }
        }
        stage('Words Image') {
          steps {
            sh 'docker login -u=$DOCKER_USR -p=$DOCKER_PSW'
            sh 'docker build -f words/Dockerfile -t mariogelo/words:latest words/.'
          }
        }
      }
      post {
        failure {
            echo 'This build has failed. See logs for details.'
        }
      }
    }

// Push to GitHub
    stage('PUSH') {
            steps {
                    sh 'docker push mariogelo/web:latest'
                    sh 'docker push mariogelo/words:latest'
            }
            post {
                failure {

                  sh 'docker system prune -f'
                    deleteDir()
                }
            }
    }
  }
}
