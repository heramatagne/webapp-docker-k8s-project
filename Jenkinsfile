pipeline {
    agent any
    
    environment {
        AWS_DEFAULT_REGION = 'us-west-2'
        EKS_CLUSTER_NAME = 'slick-cluster'
        // ECR_REPOSITORY = 'ecr-repository'
        DOCKERFILE_PATH = 'webapp-docker-k8s-project/webapp/Dockerfile'
        DOCKER_IMAGE_TAG = 'slick'
        KUBECONFIG = credentials('your-kubeconfig-credential-id')
        DOCKER_HUB_REPO = 'herasidi/centos_webapp'
    }
    
    stages {
        // stage('Source Jenkinsfile from GitHub') {
        //     steps {
        //         // Check out the project from GitHub repository
        //         checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/heramatagne/webapp-docker-k8s-project.git']]])
        //     }
        // }
        
        stage('Build Docker Image') {
            steps {
                script {
                    // Build Docker image using Dockerfile
                    docker.build(DOCKER_HUB_REPO + ":" + DOCKER_IMAGE_TAG, DOCKERFILE_PATH)
                    // Push the built Docker image to Docker Hub
                    docker.withRegistry('https://index.docker.io/v1/', 'herasidi/centos_webapp') {
                        docker.image(DOCKER_HUB_REPO + ":" + DOCKER_IMAGE_TAG).push()
                    }
                }
            }
        }

//         stage('Create EKS Cluster') {
//             steps {
//                 script {
//                     // Create Amazon EKS cluster
//                     sh "aws eks create-cluster --name ${EKS_CLUSTER_NAME} --role-arn your-eks-role-arn --resources-vpc-config subnetIds=subnet-12345678,securityGroupIds=sg-12345678 --region ${AWS_DEFAULT_REGION}"
//                     // Wait for the cluster to be ready
//                     sh "aws eks wait cluster-active --name ${EKS_CLUSTER_NAME} --region ${AWS_DEFAULT_REGION}"
//                 }
//             }
//         }

//         stage('Deploy to EKS') {
//             steps {
//                 script {
//                     // Update kubeconfig to access the created EKS cluster
//                     sh "aws eks update-kubeconfig --name ${EKS_CLUSTER_NAME} --region ${AWS_DEFAULT_REGION}"
//                     // Apply Kubernetes deployment and service manifests
//                     sh "kubectl apply -f path/to/kubernetes/deployment2.yml"
//                     sh "kubectl apply -f path/to/kubernetes/svc.yml"
//                 }
//             }
        }
    // }
}
