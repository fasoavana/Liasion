pipeline {
    agent any
    triggers {
        // Vérifie GitHub toutes les 5 minutes pour lancer le build automatiquement
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Vérification Liaison') {
            steps {
                echo 'La liaison avec GitHub est réussie !'
                // Affiche les fichiers clonés pour être sûr que tout est là
                sh 'ls -ala' 
            }
        }

        stage('Build Docker Image') {
            steps {
                // Construction de l'image à partir du Dockerfile
                sh 'docker build -t faso01/liasion-app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
                    // Connexion sécurisée et push sur Docker Hub
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USER --password-stdin"
                    sh "docker push faso01/liasion-app:latest"
                }
            }
        }
    }

    post {
        success {
            echo 'Félicitations ! Le pipeline est terminé avec succès.'
        }
        failure {
            echo 'Le pipeline a échoué. Vérifie les logs de la console.'
        }
    }
}
