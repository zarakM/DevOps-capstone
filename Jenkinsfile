pipeline {
  agent any
  stages {
    stage('Linting the index file') {
      steps {
        sh 'tidy -q -e index.html'
      }
    }

    stage('Building image') {
      steps {
        script {
          image = docker.build('zarakmughal/capstone')
        }

      }
    }

    stage('Push image to Registry') {
      steps {
        script {
          docker.withRegistry( '', 'dockerhub' ) {
            image.push()
          }
        }

      }
    }

    stage('Create cluster configuration') {
      steps {
        withAWS(region: 'us-east-2', credentials: 'eks-credential') {
          sh 'sudo aws eks --region us-east-2 update-kubeconfig --name capops'
        }

      }
    }

    stage('testing') {
      steps {
        withAWS(region: 'us-east-2', credentials: 'eks-credential') {
          sh 'kubectl get ns'
        }

      }
    }

  }
  environment {
    image = ''
  }
}