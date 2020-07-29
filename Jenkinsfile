def image
pipeline{
    agent any
    stages {
        stage("Linting the index file"){
            steps{
                sh 'tidy -q -e index.html'
            }
        }

        stage("Building image"){
            steps{
                image = docker.build('zarakmughal/capstone')
            }
        }
    }
}