pipeline {
    agent any

    environment {
        NEXUS_URL = 'http://192.168.0.138:8081'
        NEXUS_REPO = 'go-binaries'
        APP_NAME = 'app'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Test') {
            steps {
                dir('./Dockerfile') {
                    sh 'go test .'
                }
            }
        }

        stage('Build Binary') {
            steps {
                dir('CICD/hello-world') {
                    sh '''
                        mkdir -p build
                        go build -o build/hello-world .
                        ls -lh build/hello-world
                        file build/hello-world
                    '''
                }
            }
        }

        stage('Upload to Nexus') {
            steps {
                dir('CICD/hello-world') {
                    withCredentials([
                        usernamePassword(
                            credentialsId: 'nexus-credentials',
                            usernameVariable: 'NEXUS_USER',
                            passwordVariable: 'NEXUS_PASS'
                        )
                    ]) {
                        sh '''
                            curl --fail --show-error \
                                --user "$NEXUS_USER:$NEXUS_PASS" \
                                --upload-file build/hello-world \
                                "$NEXUS_URL/repository/$NEXUS_REPO/$APP_NAME-$BUILD_NUMBER"
                        '''
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build and upload completed successfully!'
        }

        failure {
            echo 'Pipeline failed!'
        }
    }
}
