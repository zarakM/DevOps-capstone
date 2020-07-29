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
                    docker.withRegistry( '', registryCredential ) { 
                        image.push()
                    }
                }
            }
        }

    }
}