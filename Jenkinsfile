pipeline {
    agent any

    tool{
        jdk 'jdk17'

        maven 'maven3'

        environment {

            SCANNER_HOME = tool 'sonar-scanner'
        }

    }

    stages {
    stage('Get Checkout') {
                steps {
                    git branch: 'main', url: 'https://github.com/Ianyalewechi/Javanow.git'
            }
            }
              stage('Compile')

                         {
                        steps {
                            sh "mvn complie"
                        }
                         }
             stage('Test') {
                        steps {
                            sh "mvn test -DskipTest =true"
                        }
                        }
             stage('OWASP Sacn') {
                         steps {
                             dependencyCheck additionalArguments: '--scan ./',odcInstallation:'DC'
                              dependencyCheckPublisher pattern: '**/dependency-check-report.xm'

                          }
                          }
             stage('Trivy FS') {
                         steps {
                             sh "trivy fs ."
                         }
                         }
             stage('sonarqube analysis') {
                          steps {
                              withSonarQubeEnv(sonar) {
                                  sh '''$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Javanow\
                                  -Dsonar.projectkey=Javanow -Dsonar.java.binaries=.'''
                          }
                          }
                stage('Build'){
                            steps {
                               sh "mvn package -DskipTests=true ."
                           }
                       }


       }
}
