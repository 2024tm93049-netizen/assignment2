pipeline {
  agent any
  environment {
    DOCKER_REG = "docker.io/YOUR_DOCKERHUB_NAMESPACE/aceest"
    IMAGE_TAG = "${env.BUILD_NUMBER ?: 'local'}"
  }
  stages {
    stage('Checkout') { steps { checkout scm } }
    stage('Install & Test') {
      steps {
        sh 'python -m pip install --upgrade pip'
        sh 'pip install -r requirements.txt'
        sh 'pytest --junitxml=reports/junit.xml --cov=app --cov-report xml:coverage.xml'
      }
      post { always { junit 'reports/junit.xml' } }
    }
    stage('SonarQube Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'sonar-scanner'
        }
      }
    }
    stage('Build Docker') { steps { sh "docker build -t ${DOCKER_REG}:${IMAGE_TAG} ." } }
    stage('Push Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh 'echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin'
          sh "docker push ${DOCKER_REG}:${IMAGE_TAG}"
        }
      }
    }
    stage('Deploy to Kubernetes') {
      steps {
        withCredentials([file(credentialsId: 'kubeconfig', variable: 'KUBECONFIG_FILE')]) {
          sh 'mkdir -p ~/.kube'
          sh 'cp $KUBECONFIG_FILE ~/.kube/config'
          sh "kubectl set image deployment/aceest-deployment aceest=${DOCKER_REG}:${IMAGE_TAG} --record || kubectl apply -f k8s/rolling-deployment.yaml"
        }
      }
    }
  }
}
