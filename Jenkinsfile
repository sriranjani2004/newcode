pipeline {
    agent any
    tools {
        nodejs 'nodejs-20'  // Use Node.js 20
    }
 
    environment {
        NODEJS_HOME = '/Users/ariv/.nvm/versions/node/v20.18.1/bin'  // Path for Node.js 20
        SONAR_SCANNER_PATH = '/Users/ariv/Downloads/sonar-scanner-6.2.1.4610-macosx-x64/bin'
    }
 
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
 
        stage('Install Dependencies') {
            steps {
                // Set the PATH and install dependencies using npm
                sh '''
                export PATH=$NODEJS_HOME:$PATH
                npm install
                '''
            }
        }
 
        stage('Lint') {
            steps {
                // Run linting to ensure code quality
                sh '''
                export PATH=$NODEJS_HOME:$PATH
                npm run lint
                '''
            }
        }
 
        stage('Build') {
            steps {
                // Build the React app
                sh '''
                export PATH=$NODEJS_HOME:$PATH
                npm run build
                '''
            }
        }
 
        stage('SonarQube Analysis') {
            environment {
                SONAR_TOKEN = credentials('sonar-token') // Accessing the SonarQube token stored in Jenkins credentials
            }
            steps {
                // Ensure that sonar-scanner is in the PATH
                sh '''
                export PATH=$SONAR_SCANNER_PATH:$PATH
                sonar-scanner -Dsonar.projectKey=pythonproject \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://localhost:9000 \
                    -Dsonar.token=$SONAR_TOKEN
                '''
            }
        }
    }
 
    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
