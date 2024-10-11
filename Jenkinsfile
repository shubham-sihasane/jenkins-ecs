pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	stages {
		stage('GitHub'){
			steps {
				git branch: 'main', credentialsId: 'jen-git-dind', url: 'https://github.com/iQuantC/Complete_CICD_02.git'
			}
		}
		stage('Unit Test'){
			steps {
				sh 'npm test'
				sh 'npm install'		
			}
		}
		stage('Integration and E2E Tests'){
			steps {
				sh 'npm run integration-tests'
				sh 'npm run e2e-tests'
			}
		}
	
	}

}
