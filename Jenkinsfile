pipeline {
    agent any

    stages {   
        stage('Build with maven') {
            steps {
                sh 'cd SampleWebApp && mvn clean install'
            }
        }
        
             stage('Test') {
            steps {
                sh 'cd SampleWebApp && mvn test'
            }
        
            }
        stage('Code Qualty Scan') {

           steps {
                  withSonarQubeEnv('sona_scanner') {
             sh "mvn -f SampleWebApp/pom.xml sonar:sonar"      
               }
            }
       }
        stage('Quality Gate') {
          steps {
                 waitForQualityGate abortPipeline: true
              }
        }
        stage('push to nexus') {
            steps {
   nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: 'nexus', groupId: 'SampleWebApp', nexusUrl: 'ec2-18-119-108-228.us-east-2.compute.amazonaws.com:8081/repository/maven-snapshots', nexusVersion: 'nexus2', protocol: 'http', repository: 'maven-snapshots', version: '1.0-snapshots'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
         deploy adapters: [tomcat9(credentialsId: 'tomcat_ID', path: '', url: 'http://52.15.49.20:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              

        
        }
} 
    }
}
