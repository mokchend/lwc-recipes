#!groovy

// https://www.jenkins.io/doc/book/pipeline/syntax/
pipeline {
    agent any

	stages {

		// -------------------------------------------------------------------------
		// Check out code from source control.
		// -------------------------------------------------------------------------

		stage('Project init') {			

			// steps {
			// 	//checkout scm
			// 	//git branch: 'master', url: 'https://github.com/mokchend/lwc-recipes.git'
			// 	checkout([$class: 'GitSCM', branches: [[name: '*/master']],
    		// 			userRemoteConfigs: [[url: 'https://github.com/mokchend/lwc-recipes.git']]])
			// 	echo "Check your environment variables"
				
        	// 	sh "git checkout master"
			// }		

			steps {
				// https://stackoverflow.com/questions/45399894/is-it-impossible-to-checkout-a-different-branch-in-jenkinsfile
				checkout scm
				sh """
					git config remote.origin.fetch '+refs/heads/*:refs/remotes/origin/*'
					git fetch --all
				"""
				/*
				checkout([
					$class: 'GitSCM',
					branches: scm.branches,
					extensions: scm.extensions + [[$class: 'LocalBranch'], [$class: 'WipeWorkspace']],
					userRemoteConfigs: [[url: 'https://github.com/mokchend/lwc-recipes.git']],
					doGenerateSubmoduleConfigurations: false
				]) */
			}	
		}


		stage('Boot Camp') {
			steps {
				command "cat ./asciiart/bunny.txt"
				echo "*** That's all folks!"
			}
			
		}
	

        stage('Development') {
            when {
                branch 'development'
            }				
            steps {
                sh 'echo "Development Stage"'
            }
        }

        stage('Integration') {
            when {
                branch 'integration'
            }				
            steps {
                sh 'echo "Integration Stage"'
            }
        }

        stage('Acceptance') {
            when {
                branch 'acceptance'
            }			
            steps {
                sh 'echo "Acceptance Stage"'
            }
        }

        stage('Production/Master') {
            when {
    			branch 'master'
            }	
            steps {
                sh 'echo "Production/master Stage"'
            }
        }

		stage('Cleaning up') {
            steps {
                sh 'echo "cleansing steps to be always executed"'
            }
        }

		

	
    }
}

def command(script) {
    if (isUnix()) {
        return sh(returnStatus: true, script: script);
    } else {
		return bat(returnStatus: true, script: script);
    }
}

def loadEnvironmentVariables(path){
    def props = readProperties  file: path
    keys= props.keySet()
    for(key in keys) {
        value = props["${key}"]
        env."${key}" = "${value}"
    }
} 
