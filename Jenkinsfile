pipeline{
    agent any
    environment{
        image = ''
    }
    stages {
        stage("Linting the index file"){
            steps{
                sh 'tidy -q -e index.html'
            }
        }

        stage("Building image"){
            steps{
                script{
                    image = docker.build('zarakmughal/capstone')
                }
            }
        }

        stage('Push image to Registry'){
            steps{
                script{
                    docker.withRegistry( '', 'dockerhub' ) { 
                        image.push()
                    }
                }
            }
        }

        stage('Create kubernetes cluster') {
			steps {
				withAWS(region:'us-east-2', credentials:'eks-credential') {
					sh '''
						eksctl create cluster \
						--name capstonecluster \
						--version 1.13 \
						--nodegroup-name standard-workers \
						--node-type t2.small \
						--nodes 2 \
						--nodes-min 1 \
						--nodes-max 3 \
						--node-ami auto \
						--region us-east-1 \
						--zones us-east-1a \
						--zones us-east-1b \
						--zones us-east-1c \
					'''
				}
			}
		}
    }
}