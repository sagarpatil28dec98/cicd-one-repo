pipeline {
  agent any

  environment {
    DOCKER_IMAGE = "sagarpatil98/cicd-demo"
  }

  stages {

    stage('Clean Workspace') {
      steps {
        deleteDir()
      }
    }

    stage('Checkout') {
      steps {
        git branch: 'devops',
            url: 'https://github.com/sagarpatil28dec98/cicd-one-repo.git'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh '''
          cd app
          docker build -t $DOCKER_IMAGE:$BUILD_NUMBER .
        '''
      }
    }

    stage('Docker Login') {
      steps {
        withCredentials([usernamePassword(
          credentialsId: 'dockerhub-creds',
          usernameVariable: 'DOCKER_USER',
          passwordVariable: 'DOCKER_PASS'
        )]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
          '''
        }
      }
    }

    stage('Push Docker Image') {
      steps {
        sh '''
          docker push $DOCKER_IMAGE:$BUILD_NUMBER
        '''
      }
    }

    stage('Update K8s Manifest') {
      steps {
        sh '''
          sed -i "s|IMAGE_TAG|$BUILD_NUMBER|g" k8s/deployment.yaml
        '''
      }
    }

    stage('Commit & Push Manifest') {
      steps {
        sh '''
          git config user.email "jenkins@ci.com"
          git config user.name "jenkins"

          git add k8s/deployment.yaml
          git commit -m "ci: update image tag to $BUILD_NUMBER" || echo "No changes to commit"
          git push origin devops
        '''
      }
    }
  }
}
