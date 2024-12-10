pipeline{
	agent{
		label "node"
	}
	stages{
		stage("clone code"){
			steps{
				git branch: 'main', credentialsId: 'Git-token_personal_account', url: 'https://github.com/DeepDN/Nodejs-CICD-pipeline.git'
			}
		stage("unit test"){
			steps{
				sh '''npm test
				npm install'''
			}	
			
		}
	}
	
}

}