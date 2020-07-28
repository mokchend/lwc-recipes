pipeline {
    // Pipeline is designed to easily use Docker images as the execution environment for a single Stage or the entire Pipeline.
    agent {
        // ERROR when : echo "*** Starting agent"
        // Must be one of [any, docker, dockerfile, kubernetes, label, none]
        // https://www.jenkins.io/doc/book/pipeline/docker/


        // TODO: this might not be the best path experience to always dynamicall create the BIG image for every commit !!!
        // better ssh into salesforce-dx container
        docker {
            image 'chendamok/salesforce-dx:latest'
            args '-v /mnt/v/docker-persist-datas/users/home/salesforce:/home/salesforce'
            args '-v /mnt/v/data01:/data01'
            args '-v  $HOME/code/dotfiles:/workspace'
            //args '-v ../../envs:/workspace/config'
            //args '-p 3000:3000 -p 5000:5000' 
        }
        
    }
    
    
    //agent any  

    //   agent {
    //     dockerfile {
    //       filename "/home/code/dotfiles/dockers/salesforce/Dockerfile"
    //       //‘Jenkins’ doesn’t have label ‘my-internal-salesforce-dx’
    //       //label "my-internal-salesforce-dx"
    //       label "salesforce-dx"
    //     }
    //   }    
    
    
    
    // environment {
    //     CI = 'true'
    // }
    stages {
        stage('Environment variables & sanity checks') {
            steps {

                // The container is not DEFINED as we need to share deamon between dind and host    

                script {
                    //Cannot connect to the Docker daemon at tcp://localhost:2375. Is the docker daemon running?
                    //docker.withServer('tcp://localhost:2375') {
                        // Error response from daemon: pull access denied for salesforce-dx, repository does not exist 
                        // or may require 'docker login': denied: requested access to the resource is denied
                        //docker.image('salesforce-dx').inside {
                            // The DinD will execute theses command below
                            sh 'sfdx force'
                            sh 'sfdx --version'
                            sh 'sfdx force:auth:list'
                            input message: 'Finished using the web site? (Click "Proceed" to continue)'
                        //}                
                    //}
                }
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

        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                //sh './jenkins/scripts/deliver-for-development.sh'
                echo "*** Development branch"
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
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
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
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
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
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
                //input message: 'Finished using the web site? (Click "Proceed" to continue)'
                //sh './jenkins/scripts/kill.sh'
            }
        }


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
    }
}