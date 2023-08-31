pipeline{
agent any

	tool {
	  maven "3.8.1"
	}

stages {
     stage('Build the application'){
        steps {
		  sh "mvn clean install"
		}
	}
    stage('Build the Docker Image'){
	  steps {
		  sh "docker build -t ikedi/demo:${BUILD_NUMBER} ."
		}
	}

  stage('Login to Docker ECR'){
	  steps {
		  withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
                              sh "docker login -u ikedi -p ${Dockerpwd}"
                          }

          		}
		}
	}

  stage('Push the Docker Image to Docker ECR'){
	  steps {
		  sh "docker push ikedi/demo:${BUILD_NUMBER}"
		}
	}

    stage('Run the Application'){
	  steps {
		  sh "docker run -p 8080:8080 ikedi/demo:${BUILD_NUMBER}"
		}
	}

}