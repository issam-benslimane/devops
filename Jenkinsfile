pipeline {
    agent any

    environment {
        registry = "issambenslimane/tp4"
        registryCredential = 'dockerhub'
    }

    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/kelguemmat/tp4devops.git'
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
                    // Add your test commands here (e.g., running a test container)
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
    }
}