pipeline {
    agent any
     tools {
         maven 'mavenHome'
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
        stage('deploy-to-staging'){
            steps {
                build job: 'deploy-to-staging'
            }
        }
    }
}
