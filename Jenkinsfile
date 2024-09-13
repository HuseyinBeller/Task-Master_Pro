@Library('my-shared-library') _

pipeline {
  agent any

  stages {
    stage('Git Checkout') {
      steps {
          script {
          gitCheckout(
            branch: 'main',
            url: 'https://github.com/HuseyinBeller/Task-Master_Pro.git'
          )
          }
      }

    stage('Unit Test Maven') {
      steps {
          script {
            mvnTest()
          }
      }
    }
    }
  }
