pipeline {
    agent any

    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'centos-app-cluster'
        DOCKERFILE_PATH = 'webapp' // Relative path to your Dockerfile from the workspace
        DOCKER_IMAGE_TAG = 'v4'
        DOCKER_HUB_REPO = 'herasidi/centos_webapp' // Define your Docker Hub repository name
        GIT_REPO_URL = 'https://github.com/heramatagne/webapp-docker-k8s-project.git' // Define your GitHub repository URL
        MANIFESTS_PATH = '.' // Specify the path to your manifest files (relative to workspace)
        DEPLOYMENT_YAML_PATH = 'deployment2.yml' // Relative path to deployment YAML file
        SERVICE_YAML_PATH = 'svc.yml' // Relative path to service YAML file
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo "Cloning the code"
                    git url: "${GIT_REPO_URL}", branch: 'main'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    echo "Building Docker image"
                    // Build Docker image using Dockerfile from specified path
                    def dockerImage = docker.build("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}", "${DOCKERFILE_PATH}")
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
                        dockerImage.push()
                    }
                }
            }
        }

//         stage('Deploy to EKS') {
//             steps {
//                 script {
//                     echo "Deploying to EKS"
//                     // Update kubeconfig for the EKS cluster
//                     sh "aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
//                     // Apply deployment and service YAML files to the default namespace
//                     sh "kubectl apply -f ${DEPLOYMENT_YAML_PATH}"
//                     sh "kubectl apply -f ${SERVICE_YAML_PATH}"
//                 }
//             }
//         }
//     }
// }

        stage('Deploy to EKS') {
            steps {
                script {
                    // Update kubeconfig for the EKS cluster
                    sh "aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
                    // Apply deployment and service YAML files to the default namespace
                    sh "kubectl apply -f ${DEPLOYMENT_YAML_PATH}"
                    sh "kubectl apply -f ${SERVICE_YAML_PATH}"
                }
            }
        }
    }
}
