pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'ec2-54-157-0-207.compute-1.amazonaws.com', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'ec2-18-234-116-105.compute-1.amazonaws.com', description: 'Production Server')
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
                      sh "scp  /var/lib/jenkins/workspace/FullyAutomated/webapp/target/webapp.war ubuntu@${params.tomcat_dev}:/home/ubuntu/tomcat/apache-tomcat-8.5.34/webapps/"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp **/target/*.war ubuntu@${params.tomcat_prod}:/home/ubuntu/tomcat/apache-tomcat-8.5.34/webapps"
                    }
                }
            }
        }
    }
}
