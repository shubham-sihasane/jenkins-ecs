pipeline{
	agent any
	// tools nodejs "NodeJS"
	
	stages{
		stage ("Clone Code"){
			steps{
				git branch: 'main', credentialsId: 'Git-token_personal_account', url: 'https://github.com/DeepDN/Nodejs-CICD-pipeline.git'
			}
		stage ("Unit Test"){
			steps{
				sh '''npm test
				npm install'''
				}
			
			}
		}
	}

}