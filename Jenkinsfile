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

        stage('Quality Gate') {
            steps {
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage('Build Docker Image on Docker Server') {
            steps {
                sshagent(['docker-ssh']) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no ubuntu@100.25.43.216 "
                    rm -rf ~/static-site &&
                    git clone ${GIT_URL} static-site &&
                    cd static-site &&
                    docker build -t static-site:latest .
                    "
                    '''
                }
            }
        }
    }
}
