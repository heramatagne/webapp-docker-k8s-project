pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'slick-cluster'
        // ECR_REPOSITORY = 'ecr-repository'
        DOCKERFILE_PATH = '//var/lib/jenkins/workspace/slickapp-pipeline/webapp' // Specify the path to your Dockerfile
        DOCKER_IMAGE_TAG = 'slick'
        // KUBECONFIG = credentials('your-kubeconfig-credential-id')
        DOCKER_HUB_REPO = 'herasidi/centos_webapp' // Define your Docker Hub repository name
        GIT_REPO_URL = 'https://github.com/heramatagne/webapp-docker-k8s-project.git' // Define your GitHub repository URL
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'pwd' // Print the current directory
                    sh 'ls'
                    // Build Docker image using Dockerfile from specified path
                    docker.build("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}", "${DOCKERFILE_PATH}")
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'herasidi') {
                        docker.image("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
