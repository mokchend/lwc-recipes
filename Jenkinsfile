#!groovy

pipeline {
    agent any

	// def toolbelt = tool 'toolbelt'
	// agent any
	// https://www.jenkins.io/doc/book/pipeline/syntax/

	stages {

		// -------------------------------------------------------------------------
		// Check out code from source control.
		// -------------------------------------------------------------------------

		stage('checkout source') {			
			steps {
                checkout scm
            }
		}


		stage('Boot Camp') {
			steps {
				command "cat ./asciiart/bunny.txt"
			}
			
		}

        stage('Development') {
            steps {
                sh 'echo "Development Stage"'
            }
        }

        stage('Integration') {
            steps {
                sh 'echo "Integration Stage"'
            }
        }

        stage('Acceptance') {
            steps {
                sh 'echo "Acceptance Stage"'
            }
        }

        stage('Production') {
            steps {
                sh 'echo "Production Stage"'
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
