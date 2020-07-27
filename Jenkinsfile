pipeline {
    // Pipeline is designed to easily use Docker images as the execution environment for a single Stage or the entire Pipeline.
    agent {
        // ERROR when : echo "*** Starting agent"
        // Must be one of [any, docker, dockerfile, kubernetes, label, none]
        docker {
            image 'chendamok/salesforce-dx:latest'
            //args '-p 3000:3000 -p 5000:5000' 
        }
    }
    // environment {
    //     CI = 'true'
    // }
    stages {
        stage('Environment variables & sanity checks') {
            steps {
                sh 'sfdx force'
                sh 'sfdx --version'
            }
        }        
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
        // stage('Deliver for development') {
        //     when {
        //         branch 'development'
        //     }
        //     steps {
        //         //sh './jenkins/scripts/deliver-for-development.sh'
        //         echo "*** Development branch"
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         //sh './jenkins/scripts/kill.sh'
        //     }
        // }
        // stage('Deliver for integration') {
        //     when {
        //         branch 'integration'
        //     }
        //     steps {
        //         //sh './jenkins/scripts/deliver-for-development.sh'
        //         echo "*** Integration branch"
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         //sh './jenkins/scripts/kill.sh'
        //     }
        // }      
        // stage('Deliver for Acceptance') {
        //     when {
        //         branch 'acceptance'
        //     }
        //     steps {
        //         //sh './jenkins/scripts/deliver-for-development.sh'
        //         echo "*** Integration branch"
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         //sh './jenkins/scripts/kill.sh'
        //     }
        // }          
        stage('Deploy for master') {
            when {
                branch 'master'
            }
            steps {
                //sh './jenkins/scripts/deploy-for-production.sh'
                echo "*** Master branch"
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }
        // stage('Deploy for production') {
        //     when {
        //         branch 'production'
        //     }
        //     steps {
        //         //sh './jenkins/scripts/deploy-for-production.sh'
        //         echo "*** Production branch"
        //         input message: 'Finished using the web site? (Click "Proceed" to continue)'
        //         //sh './jenkins/scripts/kill.sh'
        //     }
        // }
    }
}