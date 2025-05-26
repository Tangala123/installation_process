pipeline {
    agent any

    environment {
        AWS_REGION = 'eu-west-1'               // Your AWS region
        ECR_REPOSITORY = 'my-ecr-repo'         // Your ECR repo name
        IMAGE_NAME = 'my-app-repo'              // Local Docker image name
        IMAGE_TAG = 'latest'                    // Docker image tag
        AWS_ACCOUNT_ID = '323044913448'         // Your AWS Account ID
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: 'Git-Hub', url: 'https://github.com/Tangala123/my_project.git'
            }
        }

        stage('Build artifact') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Authenticate Docker to ECR') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'AWS'
                ]]) {
                    sh """
                        aws ecr get-login-password --region ${AWS_REGION} | \
                        docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com
                    """
                }
            }
        }

        stage('Tag Docker Image') {
            steps {
                sh """
                    docker tag ${IMAGE_NAME}:${IMAGE_TAG} \
                    ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}
                """
            }
        }

        stage('Push Docker Image to ECR') {
            steps {
                sh """
                    docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_REGION}.amazonaws.com/${ECR_REPOSITORY}:${IMAGE_TAG}
                """
            }
        }

        stage('Deploy to EKS') {
            steps {
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'AWS'
                ]]) {
                    sh """
                        aws eks --region ${AWS_REGION} update-kubeconfig --name my-eks-cluster
                        kubectl apply -f deployment.yaml
                        kubectl apply -f service.yaml
                    """
                }
            }
        }
    }
}
