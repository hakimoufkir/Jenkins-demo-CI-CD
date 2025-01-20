pipeline {
    environment {
        registry = "abdelhakimoufkir/tp4"
        registryCredential = 'docker-hub-token'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token',
                    url: 'https://github.com/hakimoufkir/Jenkins-demo-CI-CD.git'
            }
        }
        stage('Building Image') {
            steps {
                script {
                    dockerImage = docker.build("${registry}:latest")
                }
            }
        }
        stage('Publish Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    // Use `sh` for Linux/Unix systems
                    sh "docker run -d --name tp4_container -p 5000:80 ${registry}:latest"
                }
            }
        }
    }
}
