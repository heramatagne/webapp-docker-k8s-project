pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'slick-cluster'
        DOCKERFILE_PATH = '/var/lib/jenkins/workspace/slickapp-pipeline/webapp' // Specify the path to your Dockerfile
        DOCKER_IMAGE_TAG = 'slick'
        DOCKER_HUB_REPO = 'herasidi/centos_webapp' // Define your Docker Hub repository name
        GIT_REPO_URL = 'https://github.com/heramatagne/webapp-docker-k8s-project.git' // Define your GitHub repository URL
    }
    
    stages {
        stage('Check Docker Build Context') {
            steps {
                script {
                    sh 'pwd' // Print the current directory
                    sh 'ls' // List all files and directories in the current directory
                    sh 'ls -l' // List detailed information about files and directories in the current directory
                    sh 'ls -l webapp' // List detailed information about files and directories in the webapp directory
                    sh 'ls -a' // List all files (including hidden) and directories in the current directory
                    sh 'ls -a | grep ".dockerignore"' // Check if .dockerignore file exists
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using Dockerfile from specified path
                    docker.build("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}", "${DOCKERFILE_PATH}")
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        docker.image("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
