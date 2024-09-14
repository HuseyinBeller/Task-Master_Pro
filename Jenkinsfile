@Library('my-shared-library') _

pipeline {
  agent any

  parameters {
    choice(name: 'action', choices: 'create\ndelete', description: 'Choose Create/Destroy')
    string(name: 'ImageName', description: 'name of the Docker build', defaultValue: 'javapp')
    string(name: 'ImageTag', description: 'tag of the Docker build', defaultValue: 'v1')
    string(name: 'DockerHubUser', description: 'name of the Application', defaultValue: 'huseyinbeller')
  }

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
    stage('Quality Gate Status Check: SonarQube') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              def SonarQubecredentialsId = 'sonarqube-api'
              QualityGateStatus(SonarQubecredentialsId)
          }
      }
    }

    stage('Maven Build : Maven') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              mvnBuild()
          }
      }
    }

    stage('Docker Image Build') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              dockerBuild("${params.ImageName}", "${params.ImageTag}", "${params.DockerHubUser}")
          }
      }
    }

    stage('Nexus') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
            nexusUtils.uploadToNexus(
                nexusUrl: '3.126.121.180:8081',
                nexusVersion: 'NEXUS3',
                protocol: 'http'
                repository: 'demoapp-release',
                credentialsId: 'nexus-auth',
                groupId: 'com.example.todo',
                artifactId: 'todo-app',
                version: '1.0.0',
                file: 'target/todo-app.jar'
                type: 'jar'
            )
          }
      }
    }

    stage('Docker Image Scan: Trivy') {
        when { expression {  params.action == 'create' } }
      steps {
          script {
              dockerImageScan("${params.ImageName}", "${params.ImageTag}", "${params.DockerHubUser}")
          }
      }
    }
  }
}
