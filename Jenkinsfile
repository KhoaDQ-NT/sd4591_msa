pipeline {
    agent any
    environment {
        ecrRepo = '359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops'
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
                    sh "aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin ${ecrRepo}"
                    sh "docker push ${ecrRepo}/backend:latest"
                    sh "docker push ${ecrRepo}/frontend:latest"
                }
            }
        }
    }
}
