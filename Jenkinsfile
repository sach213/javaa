pipeline {
    agent any  // Runs on any available agent

    environment {
        DOCKER_IMAGE = "my-spring-boot-app"  // Docker image name
       // DOCKER_REGISTRY = "docker.io"        // Docker registry (can be Docker Hub or private registry)
        IMAGE_TAG = "latest"                 // Image tag for Docker
    //    DOCKER_CREDENTIALS = "docker-creds"  // Jenkins credentials for Docker login (configured in Jenkins)
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                script {
                    // Run Maven build to create the JAR file
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using Dockerfile
                    sh "docker build -t ${SACHIN}:latest ."
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    // Login to Docker registry
                    withCredentials([usernamePassword(credentialsId: DOCKER_CREDENTIALS, usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "docker login -u ${DOCKER_USERNAME} -p ${DOCKER_PASSWORD} ${DOCKER_REGISTRY}"
                    }

                    // Push the Docker image to the registry
                    sh "docker push ${SACHIN}:latest"
                }
            }
        }

        stage('Deploy to Docker') {
            steps {
                script {
                    // Deploy the Docker container (assuming the deployment environment is set up)
                    sh "docker run -d -p 8080:8080 ${DOCKER_REGISTRY}/${DOCKER_IMAGE}:${IMAGE_TAG}"
                }
            }
        }
    }

    post {
        always {
            // Clean up any Docker containers or images after the pipeline runs
            sh "docker system prune -f"
        }
    }
}
