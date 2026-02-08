pipeline {
    agent any
    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
       
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t faso01/liasion-app:latest .'
            }
        }
        
        stage('Vérification Liaison') {
            steps {
                // On ne remet PAS la commande git ici car Jenkins l'a déjà fait !
                echo 'La liaison avec GitHub est réussie !'
                sh 'ls -ala' 
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
