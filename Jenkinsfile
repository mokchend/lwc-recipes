pipeline {
    // Pipeline is designed to easily use Docker images as the execution environment for a single Stage or the entire Pipeline.
    //agent {
        // ERROR when : echo "*** Starting agent"
        // Must be one of [any, docker, dockerfile, kubernetes, label, none]
        // https://www.jenkins.io/doc/book/pipeline/docker/


        // TODO: this might not be the best path experience to always dynamicall create the BIG image for every commit !!!
        // better ssh into salesforce-dx container
        // docker {
        //     image 'chendamok/salesforce-dx:latest'
        //     args '-v /mnt/v/docker-persist-datas/users/home/root_salesforce:/root'
        //     args '-v /mnt/v/data01:/data01'
        //     //args '-p 3000:3000 -p 5000:5000' 
        // }
                  
      //- ../../envs:/workspace/config
      //- /mnt/v/data01:/data01      
      //- /var/run/docker.sock:/var/run/docker.sock
        
    //}
    agent any  
    // environment {
    //     CI = 'true'
    // }
    stages {
        stage('Environment variables & sanity checks') {
            steps {



                script {
                    //Cannot connect to the Docker daemon at tcp://localhost:2375. Is the docker daemon running?
                    //docker.withServer('tcp://localhost:2375') {
                        docker.image('salesforce-dx').inside {
                            sh 'sfdx force'
                            sh 'sfdx --version'
                            input message: 'Finished using the web site? (Click "Proceed" to continue)'
                        }                
                    //}
                }



                input message: 'Finished checking ? (Click "Proceed" to continue)'
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