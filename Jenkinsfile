pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/makarhk-sketch/sdvps-materials.git'
            }
        }

        stage('Test') {
            steps {
                sh '''
                    cd ./go.mod
                    go test .
                '''
            }
        }

        stage('Build Docker') {
            steps {
                sh '''
                    cd ./Dockerfile
                    docker build . -t netology-app:${BUILD_NUMBER}
                '''
            }
        }
    }
}
