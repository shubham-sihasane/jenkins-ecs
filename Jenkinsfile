pipeline {
  agent any

  tools {
    nodejs 'nodejs24'
  }

  environment {
    SCANNER_HOME = tool 'sonarscanner'
    APP_NAME = "ElasticApp"
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
    stage('Sonarqube Analysis') {
      steps {
        withSonarQubeEnv('sonarserver') {
          sh '''
            $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=$APP_NAME -Dsonar.projectKey=$APP_NAME -Dsonar.sources=.
            echo $SCANNER_HOME
          '''
        }
      }
    }
    stage("Quality Gate") {
      steps {
        timeout(time: 1, unit: 'HOURS') {
          waitForQualityGate abortPipeline: false
        }
      }
    }
  }

}


        