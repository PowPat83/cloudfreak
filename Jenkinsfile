pipeline {
  agent any
    tools {
      maven 'maven3'
                 jdk 'jdk11'
    }
    stages {      
        stage('Build maven ') {
            steps { 
                    sh 'pwd'      
                    sh 'mvn  clean install package'
            }
        }
        
        stage('Copy Artifact') {
           steps { 
                   sh 'pwd'
		   sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Build docker image') {
           steps {
               script {
				 docker login				 
                 def customImage = docker.build('buzz83sg76/cloudfreak', "./docker")
				 docker tag spring-petclinic baowow/petclinic:latest
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push("${env.BUILD_NUMBER}")
                 }                     
           }
        }
	  }
    }
}
