pipeline {
    agent any
    parameters {
        string(name: 'tomcat_dev', defaultValue: '18.208.183.160', description: 'staging server')
        string(name: 'tomcat_prod', defaultValue: '54.175.36.58', description: 'production server')
    }
    
	tools{
      maven 'localMaven'
    }

    triggers {
        pollSCM('* * * * *')
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -i /users/gopikrishhnachokkalamani/downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -i /users/gopikrishhnachokkalamani/downloads/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
