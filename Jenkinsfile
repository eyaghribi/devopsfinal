pipeline{

      agent any
        tools {
     jdk 'JAVA_HOME'
     maven 'M2_HOME'
  }


        stages{


                           stage('Test unitaires + mock') {
                              steps {
                               script {
                                sh 'echo "Test is processing ...."'
                                sh 'mvn clean test'
                               }

                              }

                            }

              
		stage("build jar") {
            steps {
                script {
                    sh "mvn package -DskipTests=true"
                }
            }
        }
	        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t eyaghribi/projetdev .'
                }
            }
        }
	        stage('Push image to Hub'){
            steps{
                script{
		withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u eyaghribi -p $dockerhubpwd docker.io'
}

                   sh 'docker push eyaghribi/projetdev:latest'
                }
            }
        }

	 stage('sonnarqube(qualite de code'){
                  steps{
                      script{
			      withSonarQubeEnv('sonar') {
			      sh "mvn compile sonar:sonar"
                       	     	}
			      timeout(time: 1, unit: 'HOURS') {
			      def qg = waitForQualityGate()
				      if (qg.status != 'OK') {
					   error "Pipeline aborted due to quality gate failure: ${qg.status}"
				      }
                    		}
		    	    sh "mvn clean install"

                 	}
               	 }
              }

}
}
