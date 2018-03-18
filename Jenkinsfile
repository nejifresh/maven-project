pipeline{
    /* A declarative pipeline */
    agent any
    tools{
        maven 'LocalMaven'
    }
        parameters{
            string(name: 'tomcat_dev', defaultValue: '54.234.43.113', description: 'Dev Tomcat installation')
            string(name: 'tomcat_prod', defaultValue: '35.171.4.25', description: 'Prod Tomcat installation')
            
        }

        triggers{
            pollSCM('* * * * *')
        }

        stages{
            stage('Build'){
                steps{
                    sh 'mvn clean package'
                }
                post{
                    success{
                        echo 'Now Archiving'
                        archiveArtifacts artifacts: '**/target/*.war'
                    }
                }
            }

            stage('Deployments'){
               parallel{
                   stage('Deploy to Staging'){
                       steps{
                           sh "scp -i /Users/nejitawo/Documents/tomcat.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                       }
                   }

                   stage('Deploy to Production'){
                       steps{
                             sh "scp -i /Users/nejitawo/Documents/tomcat.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                        }
                   }
               }
            }
        }

}