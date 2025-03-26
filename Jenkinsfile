pipeline {
    agent any

    stages {
        stage('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Kulsumm890/spring-framework-petclinic.git'
            }
        }
        
        stage('build') {
            steps {
              sh '''mvn clean package
                  mvn prepare-package'''
            }
        }
        
        stage('scanning') {
            steps {withSonarQubeEnv(installationName: 'sonarqube', credentialsId: 'sonarqubetoken') {
              sh 'mvn sonar:sonar'
              }
            }
        }
       stage('nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'spring-framework-petclinic', classifier: '', file: 'target/petclinic.war', type: 'war']], credentialsId: 'nexuscredentials', groupId: 'org.springframework.samples', nexusUrl: '34.208.8.163:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'mavenrepo', version: '1.0-SNAPSHOT'
              
            }
        }
         
         stage('tomcat') {
            steps {
              sshagent(['tomcat-ubuntuID']) {
              sh 'scp target/petclinic.war ubuntu@44.246.10.17:/opt/tomcat10/webapps/'

              }
            }
        }
        
        
    }
    
    post {
        success {
            mail bcc: '', body: 'congratulations!!! its a success', cc: '', from: '', replyTo: '', subject: 'FOR SUCCESSS', to: 'tanvipty@gmail.com'
        }
    

        failure {
           mail bcc: '', body: 'sorry!! its a failure', cc: '', from: '', replyTo: '', subject: 'FOR FAILURE', to: 'tanvipty@gmail.com' 
        } 
    }
}
