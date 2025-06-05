pipeline {
  agent any

  tools {
    nodejs 'nodejs24'
  }

  stages {
    stage('Clone Repository') {
      steps {
        git branch: 'main', credentialsId: 'jen-git-dind', url: 'https://github.com/shubham-sihasane/jenkins-ecs.git'
      }
    }
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
        echo "Node dependencies installed successfully."
      }
    }
    stage('Unit Testing') {
      steps {
        sh 'npm test'
        echo "Unit testing completed successfully."
      }
    }
  }

}