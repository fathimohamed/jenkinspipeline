pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '54.157.0.207', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.234.116.105', description: 'Production Server')
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
                        sh "cp -i /home/fathi/Downloads/virginia.pem **/target/*.war ubuntu@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "cp -i /home/fathi/Downloads/virginia.pem **/target/*.war ubuntu@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
