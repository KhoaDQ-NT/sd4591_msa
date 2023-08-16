pipeline {
    agent any
    environment {
        backendEcrRepo = '359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/backend'
        frontendEcrRepo = '359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/frontend'
        awsRegion = 'ap-southeast-1'
    }
    stages {
        stage('Build Backend') {
            steps {
                dir('backend') {
                    script {
                        sh 'docker build -t 359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/backend:latest .'
                    }
                }
            }
        }
        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    script {
                        sh 'docker build -t 359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/frontend:latest .'
                    }
                }
            }
        }
        stage('Push to ECR') {
            steps {
                withAWS(region: 'ap-southeast-1', credentials: 'AWS Cred') {
                    sh "aws ecr get-login-password --region ap-southeast-1 | sudo docker login --username AWS --password-stdin ${backendEcrRepo}"
                    sh "docker push ${backendEcrRepo}:latest"
                    sh "aws ecr get-login-password --region ap-southeast-1 | sudo docker login --username AWS --password-stdin ${frontendEcrRepo}"
                    sh "docker push ${frontendEcrRepo}:latest"
                }
            }
        }
    }
}
