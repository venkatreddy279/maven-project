  pipeline {
    agent any
    tools {
        maven 'localMaven'
    }
    stages{
        stage('Build'){
            steps {
                Build 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}