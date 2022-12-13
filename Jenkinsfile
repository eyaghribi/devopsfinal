pipeline{

      agent any
        tools {
     jdk 'JAVA_HOME'
     maven 'M2_HOME'
  }


        stages{
 
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
/*stage('Test unitaires + mock') {
                              steps {
                               script {
                                sh 'echo "Test is processing ...."'
                                sh 'mvn clean test'
                               }

                              }

                            }*/
		stage('docker compose'){
            steps{
                
                   sh 'docker-compose up -d'
                }
            
        }
		 stage("Sonar") {
            steps {
                sh 'mvn sonar:sonar -Dsonar.login="admin" -Dsonar.password="sonarqube" '
            }
        }

	

}
}
