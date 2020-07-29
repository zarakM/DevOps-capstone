/* groovylint-disable-next-line CompileStatic */
pipeline {
    agent any
    environment {
        image = ''
        credential = 'eks-credential'
        reg = 'us-east-2'
    }
    stages {
        stage('Linting the index file') {
            steps {
                sh 'tidy -q -e index.html'
            }
        }

        stage('Create cluster configuration') {
            steps {
                withAWS(region: reg, credentials: credential) {
                    sh 'aws eks --region us-east-2 update-kubeconfig --name capops'
                }
            }
        }

        stage('Deploying blue deployment') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f ./bluedeployment.yaml'
                }
            }
        }

        stage('Deploying blue service') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f ./blueservice.yaml'
                }
            }
        }

        stage('Deploying green deployment') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f ./greendeployment.yaml'
                }
            }
        }

        stage('Going green') {
            steps {
                input 'Ready to redirect traffic to green deployments?'
            }
        }

        stage('Deploying green service') {
            steps {
                withAWS(region:reg, credentials: credential) {
                    sh 'kubectl apply -f ./greenservice.yaml'
                }
            }
        }
    }
}

