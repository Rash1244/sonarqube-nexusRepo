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
                  withSonarQubeEnv('sonar_scanner') {
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
         nexusArtifactUploader artifacts: [[artifactId: 'SampleWebApp', classifier: '', file: 'SampleWebApp/target/SampleWebApp.war', type: 'war']], credentialsId: '8322306b-3153-41fa-94ec-40bae226c9b7', groupId: 'SampleWebApp', nexusUrl: 'ec2-18-218-183-232.us-east-2.compute.amazonaws.com:8081/repository/maven-snapshots', nexusVersion: 'nexus3', protocol: 'http', repository: 'maven-snapshots', version: '1.0-SNAPSHOTS'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
      deploy adapters: [tomcat9(credentialsId: 'tom_cat', path: '', url: 'http://18.220.49.12:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              

        
        }
} 
    }
}
