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
    }
}	    
