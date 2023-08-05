pipeline {
    agent any

    stages {
        stage('Build and Push Docker Images') {
            steps {
                withCredentials([
                    awsCredentials(credentialsId: 'AWS Cred', accessKeyVariable: 'AWS_ACCESS_KEY_ID', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')
                ]) {
                    // Set the necessary environment variables
                    def awsRegion = 'ap-southeast-1'
                    def ecrRepo = '359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops'
                    def dockerImageTag = 'latest'
                    
                    // Build and push the Docker images
                    sh "docker build -t ${ecrRepo}/frontend:${dockerImageTag} frontend/"
                    sh "docker build -t ${ecrRepo}/backend:${dockerImageTag} backend/"
                    sh "docker push ${ecrRepo}/frontend:${dockerImageTag}"
                    sh "docker push ${ecrRepo}/backend:${dockerImageTag}"
                }
            }
        }
    }
}
