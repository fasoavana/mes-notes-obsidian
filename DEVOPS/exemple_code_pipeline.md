

pipeline {
    agent any

    stages {
        stage('Clone GitHub') {
            steps {
                // Utilisation de ton lien GitHub réel
                git branch: 'main', url: 'https://github.com/fasoavana/Liasion.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                // Utilisation de ton pseudo faso01 et d'un nom d'image (ex: liasion-app)
                sh 'docker build -t faso01/liasion-app:latest .'
            }
        }

        stage('Push to Docker Hub') {
            steps {
                // On utilise l'ID 'docker-hub-creds' que tu as créé précédemment
                withCredentials([usernamePassword(credentialsId: 'docker-hub-creds', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USER')]) {
                    sh "echo \$DOCKER_HUB_PASSWORD | docker login -u \$DOCKER_HUB_USER --password-stdin"
                    sh "docker push faso01/liasion-app:latest"
                }
            }
        }
    }
}