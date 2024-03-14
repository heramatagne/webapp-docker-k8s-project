pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'slick-cluster'
        // ECR_REPOSITORY = 'ecr-repository'
        DOCKER_IMAGE_TAG = 'slick'
        // KUBECONFIG = credentials('your-kubeconfig-credential-id')
        DOCKER_HUB_REPO = 'herasidi/centos_webapp'
    }
    
    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image from Dockerfile in the same directory as Jenkinsfile
                    docker.build("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}")
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                    }
                }
            }
        }
    }
}
