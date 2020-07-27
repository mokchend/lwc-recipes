pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000 -p 5000:5000' 
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') {
            steps {
                //sh 'npm install'
                echo "*** Build Step"
            }
        }
        stage('Test') {
            steps {
                //sh './jenkins/scripts/test.sh'
                echo "*** Test Step"
            }
        }
        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                //sh './jenkins/scripts/deliver-for-development.sh'
                echo "*** Development branch"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }
        stage('Deliver for integration') {
            when {
                branch 'integration'
            }
            steps {
                //sh './jenkins/scripts/deliver-for-development.sh'
                echo "*** Integration branch"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }      
        stage('Deliver for Acceptance') {
            when {
                branch 'acceptance'
            }
            steps {
                //sh './jenkins/scripts/deliver-for-development.sh'
                echo "*** Integration branch"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }          
        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                //sh './jenkins/scripts/deploy-for-production.sh'
                echo "*** Production branch"
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }
    }
}