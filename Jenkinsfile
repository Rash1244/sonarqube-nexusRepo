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
           deploy adapters: [tomcat9(credentialsId: 'tomcat_ID', path: '', url: 'http://52.15.49.20:8080/')], contextPath: 'myapp', war: '**/*.war'
            }   
            
        }
        
        stage('deploy to tomcat') {
          steps {
         deploy adapters: [tomcat9(credentialsId: 'tomcat_ID', path: '', url: 'http://52.15.49.20:8080/')], contextPath: 'myapp', war: '**/*.war'
             
              
              

        
        }
} 
    }
}
