pipeline {
	agent any
	tools {
		nodejs 'NodeJS'
	}
	environment {
		SONAR_PROJECT_KEY = 'complete-cicd-02'
		SONAR_SCANNER_HOME = tool 'SonarQubeScanner'
		JOB_NAME_NOW = 'cicd02'
		ECR_REPO = 'iquantawsrepo'
		IMAGE_TAG = 'latest'
		ECR_REGISTRY = '358966077154.dkr.ecr.us-east-1.amazonaws.com'
		ECS_CLUSTER = 'iquant-ecs'
		ECS_SERVICE = 'iquant-ecs-svc'
		ALB_TARGET_GROUP_ARN = 'ecs-iquant-svc-tg'
		TASK_DEFINITION_FAMILY = 'iquant-ecs-task-def'
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
		stage('SonarQube Analysis'){
			steps {
				withCredentials([string(credentialsId: 'complete-cicd-02-token', variable: 'SONAR_TOKEN')]) {
    					
					withSonarQubeEnv('SonarQube') {
    						sh """
						${SONAR_SCANNER_HOME}/bin/sonar-scanner \
						-Dsonar.projectKey=${SONAR_PROJECT_KEY} \
						-Dsonar.sources=. \
						-Dsonar.host.url=http://sonarqube-dind:9000 \
						-Dsonar.login=${SONAR_TOKEN}
						"""
					}
				}
			}
		}
		stage('Docker Image'){
			steps {
				script {
					docker.build("${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG}")
				}
			}
		}
		stage('Trivy Scan'){
			steps {
				sh 'trivy --severity HIGH,CRITICAL --no-progress --format table -o trivy-report.html image ${JOB_NAME_NOW}:latest'
			}
		}
		stage('Login to ECR'){
			steps {
				sh """
				aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 358966077154.dkr.ecr.us-east-1.amazonaws.com
				"""
			}
		}
		stage('Push Image to ECR'){
			steps {
				script {
				docker.image("${ECR_REGISTRY}/${ECR_REPO}:${IMAGE_TAG}").push()
				}
			}
		}
		stage('Register ECS Task Definition') {
            		steps {
                		sh """
                		TASK_DEF=$(aws ecs describe-task-definition --task-definition $TASK_DEFINITION_FAMILY)
                		NEW_TASK_DEF=$(echo $TASK_DEF | jq --arg IMAGE "${ECR_REGISTRY}/${ECR_REPOSITORY}:${IMAGE_TAG}" '.taskDefinition.containerDefinitions[0].image = $IMAGE')
                		aws ecs register-task-definition --cli-input-json "$NEW_TASK_DEF"
                		"""
            		}
        	}
		stage('Update ECS Service'){
			steps {
				sh """
				aws ecs update-service --cluster $ECS_CLUSTER --service $ECS_SERVICE --force-new-deployment 
				"""
			}
		}
		stage('Wait for ECS SVC'){
			steps {
				sh """
				aws ecs wait services-stable --cluster $ECS_CLUSTER --service $ECS_SERVICE
				"""
			}
		}
		stage('Load Balancer'){
			steps {
				sh """
				aws elbv2 describe-target-health --target-group-arn $ALB_TARGET_GROUP_ARN
				"""
			}
		}
	}
}
