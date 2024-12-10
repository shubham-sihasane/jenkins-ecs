pipeline{
	agent any
	
	stages{
		stage("Clone Code"){
			steps{
				git branch: 'main', credentialsId: 'Git-token_personal_account', url: 'https://github.com/DeepDN/Nodejs-CICD-pipeline.git'
			}
			
		}
	}
}