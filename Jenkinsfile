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

 			      sh 'docker run -dt --name maventesting1 -p 0.0.0.0:8086:8080 mathur2000/rahil-mathur05:$BUILD_TAG'
                    }

	    	}
		stage(" move webapps.dist to webapps ") {

		    steps {
			     
		              sh 'docker exec maventesting1 sh -c "mv /usr/local/tomcat/webapps /usr/local/tomcat/webapps10 && mv /usr/local/tomcat/webapps.dist /usr/local/tomcat/webapps"'


		    }
             
                }                
                stage("app-testing using curl") {
		     steps {
                              sh 'curl -L http://localhost:8086'
                     
                     } 

	        }

		stage("approval for production") {
		    steps {
			 script {
                                Boolean userInput = input(id: 'Proceed1', message: 'Proceed and Abort?', parameters: [[$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: 'jenkins']])
                         }

 			 echo 'userInput: ' + userInput		 
		    }   
		}

        }
}
