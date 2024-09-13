pipeline {
  agent any

  stages {
    stage('Git Checkout') {
      steps {
          script {
            git branch: 'main', url: 'https://github.com/HuseyinBeller/Task-Master_Pro.git'
          }
      }
    }
  }
}
