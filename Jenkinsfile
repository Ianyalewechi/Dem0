pipeline {
    agent any

    tools {
        maven "3.8.5"
    }

    stages {
        stage('Build the application') {
            steps {
                sh "mvn clean install"
            }
        }

        stage('Build the Docker Image') {
            steps {
                sh "sudo docker build -t ikedi/demo:${BUILD_NUMBER} ."
            }
        }

        stage('Login to Docker ECR') {
            steps {
                withCredentials([string(credentialsId: 'DockerId', variable: 'Dockerpwd')]) {
                    sh "sudo docker login -u ikedi -p ${Dockerpwd}"
                }
            }
        }

        stage('Push the Docker Image to Docker ECR') {
            steps {
                sh "sudo docker push ikedi/demo:${BUILD_NUMBER}"
            }
        }

        stage('Run the Application') {
            steps {
                sh "sudo docker run -p 8090:8080 ikedi/demo:${BUILD_NUMBER}"
            }
        }

        stage(' done ') {
                    steps {
               echo "done deploying application"
                    }
                }
    }
}