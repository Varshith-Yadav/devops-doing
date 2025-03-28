pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "varshithyadav/hello-world:latest"
        PROJECT_ID = "gke-demo-455116"  // Replace with your GCP project ID
        CLUSTER_NAME = "gke-demo"   // Replace with your GKE cluster name
        REGION = "us-central1-c"              // Replace with your GKE cluster region
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Varshith-Yadav/devops-doing.git'
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
        stage('Deploy to GKE') {
            steps {
                script {
                    // Authenticate with GCP
                    withCredentials([file(credentialsId: 'gcloud-creds', variable: 'GOOGLE_APPLICATION_CREDENTIALS')]) {
                        sh '''
                        gcloud auth activate-service-account --key-file=$GOOGLE_APPLICATION_CREDENTIALS
                        gcloud config set project $PROJECT_ID
                        gcloud container clusters get-credentials $CLUSTER_NAME --region $REGION
                        '''
                    }

                    // Deploy to GKE
                    sh '''
                    kubectl apply -f k8s-deployment.yaml
                    kubectl apply -f k8s-service.yaml
                    '''
                }
            }
        }
    }
}