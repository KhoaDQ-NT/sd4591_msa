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
        stage('Trivy Scan Backend') {
            steps {
                dir('backend') {
                    script {
                        // Scan all vuln levels
                        sh "mkdir -p reports"
                        sh "wget https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl"
                        sh "trivy filesystem --ignore-unfixed --vuln-type os,library --format template --template './html.tpl' -o reports/backend-scan.html /var/lib/jenkins/workspace/MSA_CI_Trivy/backend"
                        publishHTML target: [
                            allowMissing: true,
                            alwaysLinkToLastBuild: true,
                            keepAll: true,
                            reportDir: 'reports',
                            reportFiles: 'backend-scan.html',
                            reportName: 'Trivy Scan Backend',
                            reportTitles: 'Trivy Scan Backend'
                        ]
                    }
                }
                
            }
        }

        stage('Build Frontend') {
            steps {
                dir('frontend') {
                    script {
                        sh 'docker build -t 359145461483.dkr.ecr.ap-southeast-1.amazonaws.com/my-ecr-repo-devops/frontend:11 .'
                    }
                }
            }
        }
        stage('Trivy Scan Frontend') {
            steps {
                dir('frontend') {
                    script {
                        // Scan all vuln levels
                        sh "mkdir -p reports"
                        sh "wget https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl"
                        sh "trivy filesystem --ignore-unfixed --vuln-type os,library --format template --template './html.tpl' -o reports/frontend-scan.html /var/lib/jenkins/workspace/MSA_CI_Trivy/frontend"
                        publishHTML target: [
                            allowMissing: true,
                            alwaysLinkToLastBuild: true,
                            keepAll: true,
                            reportDir: 'reports',
                            reportFiles: 'frontend-scan.html',
                            reportName: 'Trivy Scan Frontend',
                            reportTitles: 'Trivy Scan Frontend'
                        ]
                    } 
                }
            }
        }

        stage('Push to ECR') {
            steps {
                withAWS(region: 'ap-southeast-1', credentials: 'AWS Cred') {
                    sh "aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin ${backendEcrRepo}"
                    sh "docker push ${backendEcrRepo}:latest"
                    sh "aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin ${frontendEcrRepo}"
                    sh "docker push ${frontendEcrRepo}:11"
                }
            }
        }
    }
}
