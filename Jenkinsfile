def startPort = params.env == "dev"? 30100:30200
pipeline {
    agent any
    stages {
        stage('Preparing workspace') {
            steps {
                dir('k8s_files') {
                    withCredentials([usernamePassword(credentialsId:'mysql', usernameVariable:'USER', passwordVariable:'PASS')]) {
                        sh "sed -i 's/<<nodePort>>/${startPort}/g' app-service.yaml"
                        sh "sed -i 's/<<MYSQL_DATABASE>>/test_db/g' db-configmap.yaml"
                        sh "sed -i 's/<<MYSQL_USER>>/$USER/g' db-configmap.yaml"
                        sh "sed -i 's/<<MYSQL_PASSWORD>>/$PASS/g' db-configmap.yaml"
                        sh "sed -i 's/<<BUILD_NUMBER>>/$BUILD_NUMBER/g' app-deployment.yaml"
                    }
                }
            }
        }
        stage('Deploying App') {
            steps {
                sh "ansible-playbook playbook.yaml --extra-vars 'namespace=${params.env}'"
                cleanWs()
            }
        }
    }
}
