pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'centos-app-cluster'
        // ECR_REPOSITORY = 'ecr-repository'
        DOCKERFILE_PATH = '/var/lib/jenkins/workspace/slickapp-pipeline/webapp' // Specify the path to your Dockerfile
        DOCKER_IMAGE_TAG = 'v3'
        // KUBECONFIG = credentials('your-kubeconfig-credential-id')
        DOCKER_HUB_REPO = 'herasidi/centos_webapp' // Define your Docker Hub repository name
        GIT_REPO_URL = 'https://github.com/heramatagne/webapp-docker-k8s-project.git' // Define your GitHub repository URL
        MANIFESTS_PATH = '/var/lib/jenkins/workspace/slickapp-pipeline' // Specify the path to your manifest files
        DEPLOYMENT_YAML_PATH = 'deployment2.yml'
        SERVICE_YAML_PATH = 'svc.yml'        
    }
    
    stages {
        // stage('Build Docker Image') {
        //     steps {
        //         script {
        //             sh 'pwd' // Print the current directory
        //             sh 'ls -l'
        //             // Build Docker image using Dockerfile from specified path
        //             docker.build("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}", "${DOCKERFILE_PATH}")
        //             // Push the built Docker image to Docker Hub
        //             docker.withRegistry('https://index.docker.io/v1/', 'dockerhub') {
        //                 docker.image("${DOCKER_HUB_REPO}:${DOCKER_IMAGE_TAG}").push()
        //             }
        //         }
        //     }
        // }

        stage('Deploy to EKS') {
            steps {
                script {
                    // Update kubeconfig for the EKS cluster
                    sh "aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name ${EKS_CLUSTER_NAME}"
                    // Create the namespace if it doesn’t exist
                    sh "kubectl create namespace ${K8S_NAMESPACE} --dry-run=client -o yaml | kubectl apply -f -"
                    // Apply deployment YAML
                    sh "kubectl apply -f ${DEPLOYMENT_YAML_PATH} -n ${K8S_NAMESPACE}"
                    sh "kubectl apply -f ${SERVICE_YAML_PATH} -n ${K8S_NAMESPACE}"
                }
            }
        }
    }
}
