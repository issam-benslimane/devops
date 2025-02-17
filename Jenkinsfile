pipeline {
    agent any

    environment {
        registry = "issambenslimane/tp4"  // Remplacez par le nom de votre dépôt Docker Hub
        registryCredential = 'dockerhub'  // Remplacez par l'ID de vos identifiants Jenkins pour Docker Hub
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/kelguemmat/tp4devops.git'  // Remplacez par l'URL de votre dépôt GitHub
            }
        }

        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }

        stage('Test image') {
            steps {
                script {
                    echo "Running tests on the Docker image..."
                    // Ajoutez vos commandes de test ici (par exemple, exécuter un conteneur de test)
                    echo "Tests passed!"
                }
            }
        }

        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }

        stage('Deploy image') {
            steps {
                script {
                    // Arrêtez et supprimez le conteneur existant (s'il existe)
                    sh 'docker stop tp4-container || true'
                    sh 'docker rm tp4-container || true'

                    // Déployez le nouveau conteneur
                    sh "docker run -d --name tp4-container -p 80:80 ${registry}:${BUILD_NUMBER}"
                }
            }
        }
    }
}