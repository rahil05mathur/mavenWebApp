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
                             sh 'docker build -t java-repo:$BUILD_TAG .'
                             sh 'docker tag java-repo:$BUILD_TAG mathur2000/rahil-mathur05:$BUILD_TAG'
                             }
                }
                stage("dockerlogin") {
                     steps { 
		             withCredentials([string(credentialsId: 'dokHubPassID', variable: 'dokHubPassVar')]) {
			     sh 'docker login -u mathur2000 -p ${dokHubPassVar}'
			     sh 'docker push mathur2000/rahil-mathur05:$BUILD_TAG'
			     }
		     }
		      
		}
		stage("QAT TESTING") {
		     steps {  

 			      sh 'docker run -dt --name maventesting -p 0.0.0.0:8085:8080 mathur2000/rahil-mathur05:$BUILD_TAG'
                    }

	    	}
                stage("app-testing using curl") {
		     steps {
                              sh 'curl http://13.202.136.171:8085'
                     
                     } 

	        }
        }
}
