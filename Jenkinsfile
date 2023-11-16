def COLOR_MAP = [
	'SUCCESS':'good',
	'FALIURE':'danger',
]

pipeline {
	agent any
	tools {
	    maven "MAVEN3"
	    jdk "OracleJDK8"
	}

	stages {
	    stage('Fetch code') {
            steps {
               git branch: 'main', url: 'https://github.com/hkhcoder/vprofile-project'
            }

	    }

	    stage('Build'){
	        steps{
	           sh 'mvn install -DskipTests'
	        }

	        post {
	           success {
	              echo 'Now Archiving it...'
	             //archiveArtifacts artifacts: '**/target/*.war'
	           }
	        }
	    }

	    stage('UNIT TEST') {
            steps{
                sh 'mvn test'
            }
        }
		stage('Checkstyle Analysis'){
			steps{
				sh 'mvn checkstyle:checkstyle'
			
			}
		
		
		
		}
		
		
		stage('Code Analysis'){
		
			environment {
				 sonarHome = tool 'sonar4.7'
			}
			
				steps{
					withSonarQubeEnv('sonar'){
						sh '''${sonarHome}/bin/sonar-scanner \
						-Dsonar.projectKey=vprofile \
						-Dosnar.projectName=vprofile \
						-Dsonar.projectVersion=1.0 \
						-Dsonar.sources=src \
						-Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
						-Dsonar.reportsPath=target/surefire-reports/ \
						-Dsonar.junit.reportsPath=target/jococo.exec \
						-Dsonar.checkstyle.reportPaths=target/checkstyle-result.xml 
					
					'''
			
				}
			}
		
		
		
		}
		
		stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Wait for the analysis to be processed by SonarQube
                    waitForQualityGate abortPipeline: true
                }
            }
        }
		
		stage('UploadArtifact to Nexus'){
			steps{
				nexusArtifactUploader(
					nexusVersion: 'nexus3',
					protocol: 'http',
					nexusUrl: '192.168.56.11:8081',
					groupId: 'QA',
					version:"${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
					repository: 'vprofile-repo',
					credentialsId: 'nexusLogin',
					artifacts:[ 
						[artifactId: 'vproApp',
						 file: 'target/vprofile-v2.war',
						 type: 'war'
						]
					]	
				
				)
			
			}
		
		}
		
		
	}
	post{
		always {
            echo 'Slack Notifications.'
            slackSend channel: '#jenkinscicd',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}"
	
		
		}
	
	}
}