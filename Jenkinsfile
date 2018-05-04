pipeline {
  agent any
  stages {
    stage('Get Code') {
      parallel {
        stage('Get Code') {
          steps {
            echo 'Get code first'
            sh 'echo hellow'
          }
        }
        stage('') {
          steps {
            sleep(unit: 'MINUTES', time: 2)
          }
        }
      }
    }
    stage('Test code') {
      steps {
        echo 'Testing..'
      }
    }
    stage('Deploy') {
      steps {
        echo 'Deploying....'
      }
    }
  }
  environment {
    a1 = '12'
  }
}