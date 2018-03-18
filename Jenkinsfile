pipeline{
    /* A declarative pipeline */
    agent any
    tools{
       maven 'LocalMaven'
    }
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post {
                success{
                    echo 'Now archiving'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage('Deploy-To-Staging'){
            steps{
                 build job: 'deploy-to-staging'
            }
           
        }

        stage('Deploy to Production'){
            steps{
                timeout(time:5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }
                build job: 'deploy-to-prod'
            }

            post{
                success{
                   echo 'Code deployed to production'
                }
                failure{
                    echo 'Deployment failed'
                }
            }
        }
        
    }

}