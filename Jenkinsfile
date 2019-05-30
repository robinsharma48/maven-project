pipeline {
    agent any
    tools{
		maven 'localMAVEN'
	}
parameters {
         string(name: 'tomcat_stage', defaultValue: 'localhost:8090', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'localhost:9090', description: 'Production Server')
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
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war /home/robin/Downloads/apache-tomcat-staging/webapps/"
                    }
                }

                stage ('Deploy to Production'){
                    steps {
                        sh "cp /var/lib/jenkins/workspace/FullyAutomated/webapp/target/*.war /home/robin/Downloads/apache-tomcat-prod/webapps/"
                    }
                }
            }
        }
    }
}
