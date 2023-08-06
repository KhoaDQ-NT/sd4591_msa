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
                script {
                    // Define the AWS credentials ID
                    def awsCredentialsId = 'AWS Cred'

                    // Check if the credentials with the specified ID exist
                    def awsCredentials = credentials(awsCredentialsId)

                    if (!awsCredentials) {
                        error "Could not find AWS credentials with ID: ${awsCredentialsId}"
                    }

                    // Log in to AWS ECR using the credentials
                    def ecrLogin = sh(script: "aws ecr get-login-password --region ${awsRegion}", returnStdout: true).trim()
                    sh "echo '${ecrLogin}' | docker login --username AWS --password-stdin ${ecrRepo}"

                    // Push the Docker images to ECR
                    sh "docker push ${ecrRepo}/backend:latest"
                    sh "docker push ${ecrRepo}/frontend:latest"
                }
            }
        }
    }
}
