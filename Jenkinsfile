pipeline {
    agent any
    tools{
		maven 'localMAVEN'
	}
	parameters {
		string(name: 'tomcat_dev', defaultValue: '35.154.129.27', description: 'Staging Server')
         	string(name: 'tomcat_prod', defaultValue: '13.235.76.174', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '/home/jenkins/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
			sh "whoami"
			sh "scp -i /home/jenkins/tomcat-demo.pem -o StrictHostKeyChecking=no /home/jenkins/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat/webapps"
                    }
                }

                stage ('Deploy to Production'){
                    steps {
                        sh "scp -i /home/jenkins/tomcat-demo.pem -o StrictHostKeyChecking=no /home/jenkins/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat/webapps"
                    }
                }
            }
        }
    }
}
