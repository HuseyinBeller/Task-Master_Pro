@Library('my-shared-library') _

pipeline {
  agent any

  stages {
    stage('Git Checkout') {
        when { expression {  params.action == 'create' } }
      steps {
          gitCheckout(
              branch: 'main',
              url: 'https://github.com/HuseyinBeller/Task-Master_Pro.git'
            )
      }
    }
    stage('Unit Test Maven') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              mvnTest()
          }
      }
    }
    stage('Integration Test Maven') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              mvnIntegrationTest()
          }
      }
    }

    stage('Static Code Analysis: SonarQube') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              def SonarQubecredentialsId = 'sonarqube-api'
              statiCodeAnalysis(SonarQubecredentialsId)
          }
      }
    }
  }
}

