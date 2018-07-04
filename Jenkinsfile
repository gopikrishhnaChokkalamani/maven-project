pipeline {
    agent any
    
	tools{
      maven 'localMaven'
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        	post {
        		success {
        			echo 'Now Archiving...'
        			archiveArtifacts artifacts: '**/target/*.war'
        		}
        	}
        }

        stage ('Deploy to tomcat') {
        	steps {
        		build job: 'maven-project-tomcat-deploy'
        	}
        }

	stage ('Deploy to Production'){
            steps{
                timeout(time:5, unit:'DAYS'){
                    input message:'Approve PRODUCTION Deployment?'
                }

                build job: 'maven-project-tomcat-production-deploy''
            }
            post {
                success {
                    echo 'Code deployed to Production.'
                }

                failure {
                    echo ' Deployment failed.'
                }
            }
        }
    }
}
