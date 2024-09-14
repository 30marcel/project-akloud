pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('Docker-hub')
    }
    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                def scannerHome = tool 'sonarqube'
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=web-app -Dsonar.sources=. -Dsonar.host.url=http://34.203.199.229:9000"
                }
            }
        }
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
