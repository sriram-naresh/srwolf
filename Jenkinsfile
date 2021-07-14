pipeline {
    agent any	
	tools {
        maven "maven"
    }
    stages{
      stage('BUILD'){
         steps {
                sh 'mvn clean package'
            }
        }
      stage('uploads to war to nexus'){
	        steps{
		         nexusArtifactUploader artifacts: [
                     [  
		              artifactId: 'vprofile', 
		              classifier: '', 
		              file: 'target/vprofile-v1.war', 
		              type: 'war'
		            ]
		        ],  
		        credentialsId: 'nexus3-test', 
		        groupId: 'com.visualpathit', 
		        nexusUrl: '172.31.69.136:8081', 
		        nexusVersion: 'nexus3', 
	                protocol: 'http', 
		        repository: 'vprofile-release', 
		        version: 'v1'
                } 
         }
	stage("build & SonarQube analysis") {
          node {
              withSonarQubeEnv(credentialsId: 'sonarqubetoken') {
                 sh 'mvn clean package sonar:sonar'
              }    
          }
      }
      
         stage("Quality Gate"){
           timeout(time: 1, unit: 'HOURS') {
              def qg = waitForQualityGate()
              if (qg.status != 'OK') {
                  error "Pipeline aborted due to quality gate failure: ${qg.status}"
              }
          }            
    }
}	    
