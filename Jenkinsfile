pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "varshithyadav/hello-world:latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: ''
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $DOCKER_IMAGE ."
                    echo "Docker image built successfully!"
                }
            }
        }

        stage('Push the Image') {
            steps {
                script {
                    // This handles authentication automatically
                    withDockerRegistry([credentialsId: 'docker-creds', url: '']) {
                        sh "docker push $DOCKER_IMAGE"
                    }
                }
            }
        }
    }
}