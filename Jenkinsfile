pipeline {
    agent any 
    environment {
    DOCKERHUB_CREDENTIALS = credentials('docker')
    }
    stages { 
    stage('git checkout')
        steps {
            git branch: 'main', url: 'https://github.com/maksoft121/Python-app.git'
        }
        stage('Build docker image') {
            steps {  
                sh ' docker build -t maksoft121/pythonapp:$BUILD_NUMBER .'
            }
        }
        stage('login to dockerhub') {
            steps{
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('push image') {
            steps{
                sh ' docker push vatsraj/pythonapp:$BUILD_NUMBER'
            }
        }
}
post {
        always {
            sh 'docker logout'
        }
success {
                slackSend message: "Build deployed successfully - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
            }
    failure {
        slackSend message: "Build failed  - ${env.JOB_NAME} ${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
    }
    }
}
