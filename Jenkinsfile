pipeline{
    agent any
    parameters{
        string(name:'tomcat_dev', defaultValue: 'ec2-34-219-221-136.us-west-2.compute.amazonaws.com', description: 'Staging server')
        string(name:'tomcat_prod', defaultValue: 'ec2-34-211-248-219.us-west-2.compute.amazonaws.com', description: 'Production-server')
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
                        sh "scp -i /home/laura.donoso/key_aws_blog/lara.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
                stage('Deploy yo production'){
                    steps{
                        sh "scp -i /home/laura.donoso/key_aws_blog/lara.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}