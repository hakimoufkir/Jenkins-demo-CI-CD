pipeline {
    environment {
        registry = "abdelhakimoufkir/tp4" // your dockerhub credentials (to change)
        registryCredential = 'docker-hub-token' // this credentialID is created in jenkins and
                                         //contain dockerhub credentials(to change)
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git branch: 'main',
                    credentialsId: 'github-token', // your GIT credentials (to change)
                    url: 'https://github.com/hakimoufkir/tp4.git' // your GIT repo (to change)
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
                    bat "docker run -d ${registry}:latest"
                }
            }
        }
    }
}
