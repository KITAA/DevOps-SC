pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git branch: 'main', url: 'https://github.com/KITAA/DevOps-SC.git'
      }
    }
    stage('Check Docker Installation') {
      steps {
        echo 'Checking if Docker is installed and accessible...'
        bat 'docker --version'
      }
    }
    stage('Build Docker Image') {
      steps {
        bat 'docker build -t irhamhakim02/devops-demo .'
      }
    }
    stage('Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
          bat '''
            echo %DOCKER_PASS% | docker login -u %DOCKER_USER% --password-stdin
            docker push irhamhakim02/devops-demo
          '''
        }
      }
    }
    stage('Run JMeter Test') {
      steps {
        bat 'jmeter -n -t test.jmx -l result.jtl'
      }
    }
    stage('Publish Report') {
      steps {
        perfReport sourceDataFiles: 'result.jtl'
      }
    }
  }
}
