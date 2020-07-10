pipeline {
    agent any
     tools {
         maven 'mavenHome'
         jdk 'localJDK'
    }
    stages{
        stage('Build'){
            steps {
                bash 'mvn clean package'
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
