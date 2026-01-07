pipeline {
    agent any

    stages {
        stage('Clone GitHub') {
            steps {
                git 'https://github.com/fasoavana/Liasion.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t faso01/liasion-app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USER --password-stdin"
                    sh "docker push faso01/liasion-app:latest"
                }
            }
        }
    }
}
