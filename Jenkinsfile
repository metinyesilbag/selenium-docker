pipeline {

    agent any

    stages {

        stage('Build Jar') {
            steps {
                bat 'mvn clean package -DskipTests'
            }
        }

        stage('Build Image') {
            steps {
                bat 'docker build -t=vinsdocker/selenium:latest .'
            }
        }

        stage('Push Image') {
            environment {
                DOCKER_HUB_USR = credentials('dockerhub-creds_USR')
                DOCKER_HUB_PSW = credentials('dockerhub-creds_PSW')
            }
            steps {
                bat 'echo %DOCKER_HUB_PSW% | docker login -u %DOCKER_HUB_USR% --password-stdin'
                bat 'docker push vinsdocker/selenium:latest'
                bat "docker tag vinsdocker/selenium:latest vinsdocker/selenium:${env.BUILD_NUMBER}"
                bat "docker push vinsdocker/selenium:${env.BUILD_NUMBER}"
            }
        }

    }

    post {
        always {
            bat 'docker logout'
        }
    }

}
