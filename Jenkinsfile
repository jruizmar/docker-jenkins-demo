pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
       sh 'docker build -t app .'
      }
    }
    stage('Test') {
      steps {
        echo 'TEST'
	sh 'docker run -d  --name app3 -id -p 80:80 app:latest'
	sh '/bin/nc -vz localhost 80'
	sh 'docker stop app3'
      }
      post{
        always {
	 sh 'docker container stop app3'
	}
      }
    }
    stage('Push Registry'){
      steps{     
	withDockerRegistry([ credentialsId: (credentialsId: "f143dc07-01c7-4b5b-bd0b-b8b342cb40b8", url: "" ]) {
	   sh 'docker tag app:latest jruizmar/app:stable'
	   sh 'docker push jruizmar/app:stable'
	 }
      }
    }
  }
}