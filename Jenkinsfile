pipeline {
    agent any

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        IMAGE_NAME = "devtestgit17_07"
        CONTAINER_PORT = "3001"
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/Quan1906-code/devtestgit17_07.git'
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore'
            }
        }

        stage('Test') {
            steps {
                sh 'dotnet test --no-build --verbosity normal'
            }
        }

        stage('Publish') {
            steps {
                sh 'dotnet publish -c Release -o publish'
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Docker Run') {
            steps {
                script {
                    // Stop and remove any existing container with same name
                    sh "docker rm -f ${IMAGE_NAME} || true"
                    sh "docker run -d -p ${CONTAINER_PORT}:${CONTAINER_PORT} --name ${IMAGE_NAME} ${IMAGE_NAME}:latest"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished.'
        }
        success {
            echo 'Deployment successful!'
        }
        failure {
            echo 'Deployment failed.'
        }
    }
}
