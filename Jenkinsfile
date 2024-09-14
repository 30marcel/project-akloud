pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('Docker-hub')
        SONARQUBE_CREDENTIALS = credentials('sonarqube')
    }

    stages {
        stage('Build') {
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t melong123/web-app:1.0.5 .'
                }
            }
        }
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Run SonarQube analysis
                    withCredentials([usernamePassword(credentialsId: 'sonarqube', usernameVariable: 'SONARQUBE_USERNAME', passwordVariable: 'SONARQUBE_PASSWORD')]) {
                        sh '''
                        sonar-scanner \
                          -Dsonar.projectKey=marcel-website-scan \
                          -Dsonar.host.url=http://34.203.199.229:9000 \
                          -Dsonar.login=$SONARQUBE_PASSWORD
                        '''
                    }
                }
            }
        }
        stage('Login') {
            steps {
                script {
                    // Log in to Docker Hub
                    withCredentials([usernamePassword(credentialsId: 'Docker-hub', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                        sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                    }
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    // Push the Docker image to Docker Hub
                    sh 'docker push melong123/web-app:1.0.5'
                }
            }
        }
        stage('Logout') {
            steps {
                script {
                    // Log out from Docker Hub
                    sh 'docker logout'
                }
            }
        }
    }
