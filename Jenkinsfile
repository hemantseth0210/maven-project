pipeline {
    agent any

    tools {
        maven 'localMaven'
        jdk 'localJDK'
    }

    parameters { 
         string(name: 'tomcat_dev', defaultValue: '18.191.213.165', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.191.0.161', description: 'Production Server')
    } 
 
    triggers {
         pollSCM('* * * * *') // Polling Source Control
    }


    stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
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
                         bat "winscp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "winscp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}