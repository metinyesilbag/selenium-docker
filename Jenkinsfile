pipeline {
    agent any

    environment {
        DOCKER_HUB = credentials('dockerhub-creds')
    }

    stages {
        stage('Build Jar') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image') {
            steps {
                bat 'docker build -t myesilbag/dockerhub-creds:latest .'
            }
        }

        stage('Push Image') {
            steps {
                bat "echo ${DOCKER_HUB_PSW} | docker login -u ${DOCKER_HUB_USR} --password-stdin"
                bat 'docker push myesilbag/dockerhub-creds:latest'
                bat "docker tag myesilbag/dockerhub-creds:latest myesilbag/dockerhub-creds:${env.BUILD_NUMBER}"
                bat "docker push myesilbag/dockerhub-creds:${env.BUILD_NUMBER}"
            }
        }
    }

    post {
        always {
            bat 'docker logout'
        }
    }
}
