pipeline {
  agent any
  environment {
    IMAGE = "cicd-app"
    TAG   = "${env.BUILD_NUMBER}"
  }
  stages {
    stage('Checkout') {
      steps { checkout scm }
    }
    stage('Build Docker Image') {
      steps { sh 'docker build -t $IMAGE:$TAG .' }
    }
    stage('Load Image into Kubernetes') {
      steps { sh 'kind load docker-image $IMAGE:$TAG --name devops' }
    }
    stage('Deploy') {
      steps { sh 'sed "s/IMAGE_TAG/$TAG/" k8s/deployment.yml | kubectl apply -f -' }
    }
    stage('Verify') {
      steps { sh 'kubectl rollout status deployment/cicd-app --timeout=120s' }
    }
  }
}
