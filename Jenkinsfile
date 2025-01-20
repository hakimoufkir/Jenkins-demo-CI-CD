pipeline {
    environment {
        registry = "abdelhakimoufkir/tp4" // Your Docker Hub repository
        registryCredential = 'docker-hub-token' // Your Jenkins Docker Hub credentials ID
        dockerImage = '' // Variable to hold the Docker image reference
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token', // Your Jenkins GitHub credentials ID
                    url: 'https://github.com/hakimoufkir/Jenkins-demo-CI-CD.git' // Your GitHub repository URL
            }
        }
        stage('Building Image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:latest") // Builds and tags the image
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push() // Pushes the image to Docker Hub
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    // Find existing container, remove it, and launch a new one
                    sh """
                    if [ \$(docker ps -q --filter name=tp4_container) ]; then
                        docker stop tp4_container
                        docker rm tp4_container
                    fi
                    docker run -d --name tp4_container -p 5000:80 ${registry}:latest
                    """
                }
            }
        }
    }
}
