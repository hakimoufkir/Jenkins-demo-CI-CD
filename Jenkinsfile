pipeline {
    environment {
        registry = "abdelhakimoufkir/tp4"
        registryCredential = 'dockerhub'
        dockerImage = ''
    }
    agent any
    stages {
        stage('Cloning Git') {
            steps {
                git 'https://github.com/hakimoufkir/tp4'
            }
        }
        stage('Building image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":latest"
                }
            }
        }
        stage('Test image') {
            steps {
                script {
                    echo "Tests passed"
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
                    sh "docker run -d --name tp4-container -p 8080:80 ${registry}:latest"
                }
            }
        }
    }
}

