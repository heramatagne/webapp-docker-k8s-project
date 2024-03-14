pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'slick-cluster'
        // ECR_REPOSITORY = 'ecr-repository'
        DOCKERFILE_PATH = 'webapp/Dockerfile' // Specify the path to your Dockerfile or its directory
        DOCKER_IMAGE_TAG = 'slick'
        // KUBECONFIG = credentials('your-kubeconfig-credential-id')
        DOCKER_HUB_REPO = 'herasidi/centos_webapp' // Define your Docker Hub repository name
        GIT_REPO_URL = 'https://github.com/heramatagne/webapp-docker-k8s-project.git' // Define your GitHub repository URL
    }
    
    stages {
        stage('Clone Repository') {
            steps {
                script {
                    git branch: 'main', url: GIT_REPO_URL
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using Dockerfile from cloned repository
                    docker.build("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}", webapp/Dockerfile')
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
