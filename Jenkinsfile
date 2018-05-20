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
	sh 'docker run -d  --name app -id -p 80:80 app:test'
	sh '/bin/nc -vz localhost 80'
	sh 'docker stop app'
      }
      post{
        always {
	 sh 'docker container stop app'
	}
      }
    }
    stage('Push Registry'){
      steps{     
	withCredentials([usernamePassword(credentialsId: 'f143dc07-01c7-4b5b-bd0b-b8b342cb40b8', passwordVariable: 'password', usernameVariable: 'user')]) {
           echo "PASSWORD: $password"
           echo "USER: $user
	   sh 'docker tag app:test jruizmar/app:stable'
	   sh 'docker push jruizmar/app:stable'
	 }
      }
    }
  }
}
