pipeline {
    agent any
    stages {
        stage('Building image') {
          steps {
            sh "git checkout ${params.commit_id}"
            sh  "docker build -t ${env.private_nexus}/voda-nodeapp --network host ."
          }
        }
        stage('Deploying Image') {
          steps {
            withCredentials([usernamePassword(credentialsId:"nexus",usernameVariable:"USERNAME",passwordVariable:"PASSWORD")]){
              sh "docker login ${env.private_nexus} --username $USERNAME --password $PASSWORD"
              sh "docker push ${env.private_nexus}/voda-nodeapp"
            }
          }
        }
    }
}
