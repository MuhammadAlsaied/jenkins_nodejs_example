pipeline {
    environment {
    registry = 'muhammadalsaied/voda-nodeapp'
    registryCredential = 'dockerhub'
    dockerImage = ''
    }
    agent any
    stages {
        stage('Building image') {
          steps {
            sh "git checkout ${params.commit_id}"
            sh  'docker build -t muhammadalsaied/voda-nodeapp --network host .'
          }
        }
        stage('Deploying Image') {
          steps {
            withCredentials([usernamePassword(credentialsId:"dockerhub",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]){
              sh 'docker login --username $USERNAME --password $PASSWORD'
              sh 'docker push muhammadalsaied/voda-nodeapp'
            }
          }
        }
    }
}
