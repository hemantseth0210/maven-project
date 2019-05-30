pipeline {
    agent any
    tools {
        maven 'localMaven'
        jdk 'localJDK'
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
        stage ('Running Tests'){
            steps {
                build job: 'static-analysis'
            }
        }
        stage ('Deploy to Staging'){
            steps {
                build job: 'deploy-to-stage'
            }
        }
    }
}