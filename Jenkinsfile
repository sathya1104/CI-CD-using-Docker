pipeline {
    agent any
	
	  tools
    {
       maven "Maven"
	   dockerTool "Docker"
    }
 stages {
      stage('checkout') {
           steps {
             
                git branch: 'master', url: 'https://github.com/sathya1104/CI-CD-using-Docker.git'
             
          }
        }
	 stage('Execute Maven') {
           steps {
             
                sh 'mvn package'             
          }
        }
        

  stage('Docker Build and Tag') {
           steps {
              
                sh 'docker build -t samplewebapp:latest .' 
                //sh 'docker tag samplewebapp sathya1104/samplewebapp:latest'
                sh 'docker tag samplewebapp sathya1104/samplewebapp:$BUILD_NUMBER'
                sh 'docker images'
          }
        }
     
  stage('Publish image to Docker Hub') {
          
            steps {
			def DOCKER_REGISTRY_URI="https://registry.hub.docker.com"
            withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerHub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
				sh "docker login --password=${PASSWORD} --username=${USERNAME} ${DOCKER_REGISTRY_URI}"
			}
                  
          }
  }
     
      stage('Run Docker container on Jenkins Agent') {
             
            steps 
			{
                sh "docker run -d -p 8003:8080 sathya1104/samplewebapp:$BUILD_NUMBER"
 
            }
        }
/*		
 stage('Run Docker container on remote hosts') {
             
            steps {
                sh "docker -H ssh://jenkins@172.31.28.25 run -d -p 8003:8080 sathya1104/samplewebapp"
 
            }
        }
*/		
    }
	}
    
