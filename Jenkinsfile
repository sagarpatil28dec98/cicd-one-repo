pipeline {
  agent any

  stages {
    stage('Checkout') {
      steps {
        git branch: 'dev',
            url: 'https://github.com/sagarpatil28dec98/cicd-one-repo.git'
      }
    }

    stage('Build') {
      steps {
        sh 'docker build -t sagarpatil98/cicd-one-repo:$BUILD_NUMBER .'
      }
    }
  }
}

