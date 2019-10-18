pipeline{
    agent any
    parameters{
        string(name:'tomcat_dev',defaultValue: '34.220.42.74',description: 'Staging server')
        string(name:'tomcat_prod',defaultValue: '54.213.254.58'. description: 'Production-server')
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
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
        stage ('Deployments'){
            parallel{
                stage('Deploy yo staging'){
                    steps{
                        sh "scp -i /home/laura.donoso/key_aws_blog/lara.pem **/target/*.war ec2-user@{params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage('Deploy yo production'){
                    steps{
                        sh "scp -i /home/laura.donoso/key_aws_blog/lara.pem **/target/*.war ec2-user@{params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}