pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/yourusername/devops-demo.git'
      }
    }
    stage('Build Docker Image') {
      steps {
        sh 'docker build -t yourusername/devops-demo .'
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push yourusername/devops-demo
          '''
        }
      }
    }
    stage('Run JMeter Test') {
      steps {
        sh 'jmeter -n -t test.jmx -l result.jtl'
      }
    }
    stage('Publish Report') {
      steps {
        perfReport sourceDataFiles: 'result.jtl'
      }
    }
  }
}
