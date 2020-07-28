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
            args "-v /home/mokch/code/dotfiles:/workspace" // to interprt $ sign // TODO: how to use environment variable define above ?
            //args '-v ../../envs:/workspace/config'
            //args '-p 3000:3000 -p 5000:5000' 
        }

        
    }

    environment {
        def workspace = '/home/mokch/code/dotfiles'
        SF_VERSION="41.0"
        SF_USERNAME="chenda.mok@gmail.com.devcicd"
        SF_PASSWORD="Welcome-123"
        SF_SERVERURL="https://login.salesforce.com"
        SF_TESTLEVEL="NoTestRun"
        SF_RUNTESTS=""
        SF_CHECKONLY="true"
        SF_TESTSUFFIX="_TEST"
        SF_SRC_PATH="./force-app/main/default"
        // Used for: defining the path to the repository folder
        SF_REPO_PATH="./force-app/main/default"
        POST_SCRIPT_PATH="/script-post"
        PRE_SCRIPT_PATH="/script-pre"
        INSERT_LOAD="/insert"
        UPDATE_LOAD="/update"
        UPSERT_LOAD="/upsert"
        DELETE_LOAD="/delete"

        // Here is the list of optional parameters with their default value :
        //CURRENT_BRANCH=
        //COMPARE_BRANCH=
        //CODECLIMATE_REPO_TOKEN=
        //COMMIT=
        //SF_ALLOWMISSINGFILES=true
        //SF_IGNOREWARNINGS=true
        //SF_POLLINTERVAL=5000
        //SF_POLLTIMEOUT=10000
        //SF_ROLLBACKONERROR=true
        //SF_SINGLEPACKAGE=true
        //SF_VERBOSE=true
        //SF_PROJECT=
        //SF_CUSTOMOBJECTS=true
    }    
    
    
    stages {

        stage('sfdc-ci-toolkit > initialize environment') {
        
            steps {
                
                sh "echo ${SF_VERSION}"
                echo "*** $SF_VERSION"

                sh "echo ${SF_USERNAME}"
                echo "*** $SF_USERNAME"

                sh 'cp -Rp /workspace/sfdc-ci-toolkit/ .'
                sh 'cd sfdc-ci-toolkit && npm run profile-reconciliation'

                //input message: 'Finished profile-reconciliation (Click "Proceed" to continue)'    
            }
   
        }   

        stage("sfdc-ci-toolkit: profile-completion") {
    
            steps {
                
                sh 'cd sfdc-ci-toolkit && npm run profile-reconciliation'
                input message: 'Finished profile-reconciliation (Click "Proceed" to continue)'
                
            }
            
        }

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
                            echo "Home Directory=${workspace}"
                            sh 'sfdx force'
                            sh 'sfdx --version'                            
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


    
    }
}