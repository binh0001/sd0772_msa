pipeline {
    agent none
    environment {
        REGION = "ap-southeast-2"
        FRONTEND_APP = "frontend"
        BACKEND_APP = "backend"
        ECR_URI = "547215031201.dkr.ecr.ap-southeast-2.amazonaws.com" // change
        IMAGE_TAG = "${BUILD_NUMBER}"
        GIT_REPO = "https://github.com/binh0001/sd0772_msa.git"
        REPO_NAME = "sd0772"
    }
    stages {
        stage('Docker Build Frontend') {
            agent any
            steps {
                withAWS(region:'ap-southeast-2',credentials:'aws-credential') {
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
                    sh "docker build -t ${FRONTEND_APP}:${IMAGE_TAG} src/frontend/"
                    sh "docker tag ${FRONTEND_APP}:${IMAGE_TAG} ${ECR_URI}/${FRONTEND_APP}:latest"
                    sh "docker push ${ECR_URI}/${FRONTEND_APP}:latest"
                }
            }
        }
        stage('Docker Build Backend') {
            agent any
            steps {
                withAWS(region:'ap-southeast-2',credentials:'aws-credential') {
                    sh "aws ecr get-login-password --region ${REGION} | docker login --username AWS --password-stdin ${ECR_URI}"
                    sh "docker build -t ${BACKEND_APP}:${IMAGE_TAG} src/backend/"
                    sh "docker tag ${BACKEND_APP}:${IMAGE_TAG} ${ECR_URI}/${BACKEND_APP}:latest"
                    sh "docker push ${ECR_URI}/${BACKEND_APP}:latest"
                }
            }
        }

        stage('Deploy k8s') {
            agent any
            steps {
                withAWS(region: 'ap-southeast-2', credentials: 'aws-credential') {
                    sh "aws eks update-kubeconfig --name sd1121-devops-eks"
                    sh "kubectl apply -f k8s/aws/mongodb.yaml"
                    sh "kubectl apply -f k8s/aws/backend.yaml"
                    sh "kubectl apply -f k8s/aws/frontend.yaml"
                }
            }
        }
    }
}
