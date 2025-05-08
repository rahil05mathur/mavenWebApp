pipeline {
        agent any

          
        stages {
                stage("SCM") {
                     steps {
                            git 'https://github.com/rahil05mathur/mavenWebApp.git'
                            }
                          }

                stage("build") {
                     steps {
                             sh 'mvn clean package'
                             }
                           }
                stage("build-image") {
                     steps {
                             sh 'sudo docker build -t java-repo:$BUILD_TAG .'
                             sh 'sudo docker tag java-repo:$BUILD_TAG mathur2000/rahil-mathur05:$BUILD_TAG'
                             }
                }
                stage("dockerlogin") {
                     steps { 
		             withCredentials([string(credentialsId: 'dokHubPassID', variable: 'dokHubPassVar')]) {
			     sh 'sudo docker login -u mathur2000 -p ${dokHubPassVar}'
			     sh 'sudo docker push mathur2000/rahil-mathur2000:$BUILD_TAG'
			     }
		     }
		      
		}
		stage("QAT TESTING") {
		     steps {  
		              sh 'sudo docker rm -f $(sudo docker ps -a -q)'
 			      sh 'sudo docker run -dt --name maventesting -p 8085:8080 mathur2000/rahil-mathur05:$BUILD_TAG'
                    }

	    	}
                stage("app-testing using curl") {
		     steps {
                              sh 'curl 65.2.160.227:8085'
                     
                     } 

	        }
        }
}
