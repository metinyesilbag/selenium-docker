pipeline {
    agent any

    environment {
        DOCKER_HUB_USR = 'myesilbag'
        DOCKER_HUB_PSW = '99Gfm839e.'
    }

    stages {
        stage('Build Jar') {
            steps {
                node('windows') {  // Windows etiketini kullanalım
                    bat 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Build Image') {
            steps {
                node('windows') {  // Windows etiketini kullanalım
                    bat 'docker build -t myesilbag/dockerhub-creds:latest .'
                }
            }
        }

        stage('Push Image') {
            steps {
                node('windows') {  // Windows etiketini kullanalım
                    bat "echo ${DOCKER_HUB_PSW} | docker login -u ${DOCKER_HUB_USR} --password-stdin"
                    bat 'docker push myesilbag/dockerhub-creds:latest'
                    bat "docker tag myesilbag/dockerhub-creds:latest myesilbag/dockerhub-creds:${env.BUILD_NUMBER}"
                    bat "docker push myesilbag/dockerhub-creds:${env.BUILD_NUMBER}"
                }
            }
        }
    }

    post {
        always {
            node('windows') {  // Windows etiketini kullanalım
                bat 'docker logout'
            }
        }
    }
}
