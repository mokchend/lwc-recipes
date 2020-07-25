#!groovy

// https://www.jenkins.io/doc/book/pipeline/syntax/
pipeline {
    agent any

	stages {

		// -------------------------------------------------------------------------
		// Check out code from source control.
		// -------------------------------------------------------------------------

		stage('Project init') {			
			steps {
                checkout scm
				echo "Check your environment variables"
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

        stage('Production') {
            when {
                branch 'origin/master'
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
