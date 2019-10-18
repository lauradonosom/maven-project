pipeline{
    agent any
    stages{
        stage('Build'){
            steps{
                sh 'mvn clean package'
            }
            post{
                success{
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage('Deploy to staging'){
            steps{
                build job: 'deploy-to-staging'
            }
        }
        stage('Deploying to production'){
            steps{
                timeout(time:5, unit: 'DAYS'){
                    input message: 'Approve PRODUCTION Deployment?'
                }

                build job: 'deploy-to-prod'
            }
            post{
                success{
                    echo 'Code deployed to Prodcution'
                }

                failure{
                    echo ' Deployment failed'
                }
            }
        }
    }
}