pipeline{
    agent any
    stages {
        stage("pushing"){
            steps{
                bat "tidy -q -e index.html"
            }
        }
    }
}