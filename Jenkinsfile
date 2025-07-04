pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/KITAA/DevOps-SC.git'
      }
    }

    stage('Check Docker Installation') {
      steps {
        echo 'Checking if Docker is installed and accessible...'
        sh 'docker --version'       // Shows Docker version
        sh 'docker ps'              // Checks Docker daemon access
      }
    }

    stage('Build Docker Image') {
      steps {
        echo 'Building Docker image...'
        sh 'docker build -t irhamhakim02/devops-demo .'
      }
    }

    stage('Push Docker Image') {
      steps {
        echo 'Pushing Docker image to Docker Hub...'
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          sh '''
            echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
            docker push irhamhakim02/devops-demo
          '''
        }
      }
    }

    stage('Run JMeter Test') {
      steps {
        echo 'Running JMeter performance test...'
        sh 'jmeter -n -t test.jmx -l result.jtl'
      }
    }

    stage('Publish Report') {
      steps {
        echo 'Publishing JMeter performance report...'
        perfReport sourceDataFiles: 'result.jtl'
      }
    }
  }
}
