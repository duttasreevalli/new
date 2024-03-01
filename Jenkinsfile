pipeline {
	    agent any

 
	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '1'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/sreevalli/DefaultTenant/orchestrator_/"
	        UIPATH_ORCH_LOGICAL_NAME = "sreevalli"
	        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
	        UIPATH_ORCH_FOLDER_NAME = "REFUiDemo"
	    }

 
	    stages {

 
	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranchName ${env.BRANCH_NAME}"
	                checkout scm

 
	            }
	        }

 
	         // Building Tests
	        stage('Build Tests') {
	            steps {
	                echo "Building package with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "C:\\Users\\Relanto\\Documents\\UiPath\\new\\Output\\${env.BUILD_NUMBER}",
						  outputType: 'Tests',
	                      projectJsonPath: "project.json",
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: 'None'
						)
	            }
	        }
	         // Deploy Stages
	        stage('Deploy Tests') {
	            steps {
	                echo "Deploying ${BRANCH_NAME} to orchestrator"
	                UiPathDeploy (
	                packagePath: "C:\\Users\\Relanto\\Documents\\UiPath\\new\\Output\\${env.BUILD_NUMBER}",
	                orchestratorAddress: "${UIPATH_ORCH_URL}",
	                orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
	                folderName: "${UIPATH_ORCH_FOLDER_NAME}",
	                environments: '',
	                //credentials: [$class: 'UserPassAuthenticationEntry', credentialsId: 'APIUserKey']
	                credentials: Token(accountName: "${UIPATH_ORCH_LOGICAL_NAME}", credentialsId: 'APIUserKey'),
					traceLevel: 'None',
					 createProcess: true,
					entryPointPaths: 'Main.xaml'

 
					)
	            }
			}	
	    }


     // Deploy to Production Step
	        stage('Deploy to Production') {
	            steps {
	                echo 'Deploy to Production'
	                }
	            }
	    }
	
 
	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }

 
	
 
	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	}			
