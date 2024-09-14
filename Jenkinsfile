pipeline {
    agent any
    environment{
      DOCKERHUB_CREDENTIALS = credentials('Docker-hub')
    }
    stages {
        stage('Build') {
            steps {
                sh 'docker build -t melong123/web-app:1.0.5 .'
            }
        }
        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push') {
            steps {
                sh 'docker push melong123/web-app:1.0.5'
            }
        }
        stage('Logout') {
            steps {
                sh 'docker logout'
            }
        }
    }
}
