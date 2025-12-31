pipeline {
    agent any

    environment {
        SONAR_SCANNER_HOME = tool 'SonarScanner'
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    sh '''
                    $SONAR_SCANNER_HOME/bin/sonar-scanner \
                    -Dsonar.projectKey=static-website \
                    -Dsonar.projectName=static-website \
                    -Dsonar.sources=.
                    '''
                }
            }
        }

     

        stage('Build Docker Image on Docker Server') {
            steps {
               
            }
        }
    }
}
