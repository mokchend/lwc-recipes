#!groovy

node {

	def toolbelt = tool 'toolbelt'

	



	stage('Initialisation') {
		command "cat ./asciiart/bunny.txt"
	}

    // -------------------------------------------------------------------------
    // Check out code from source control.
    // -------------------------------------------------------------------------

    stage('checkout source') {
        checkout scm
    }



	stage('update variables') {
		// dynamically set the environment properties from .env
		// .properties file way
		path = "${workspace}/.env"
		
		// ERROR: java.lang.NoSuchMethodError: No such DSL method 'readProperties'
		// require: https://plugins.jenkins.io/pipeline-utility-steps/
		readProperties(file: path).each {key, value -> env[key] = value }

		// This is working correctly
		// Groovy way but require a groovy syntax file
		// load "${env.WORKSPACE}/env-devcicd.groovy"

           // root user where home=/root
		echo "${HOME}"
		echo "${env.WORKSPACE}"
		
		
		//load "${env.WORKSPACE}/.env"
		// bash usage
		echo "${env.DB_URL}"
		// groovy usage	
		println env.DB_URL
			
		echo "${env.DB_URL2}"
		println "*** DB_URL2=" + env.DB_URL2

			def SF_CONSUMER_KEY=env.SF_CONSUMER_KEY
			def SF_USERNAME=env.SF_USERNAME
			def SERVER_KEY_CREDENTIALS_ID=env.SERVER_KEY_CREDENTIALS_ID
			def DEPLOYDIR=env.DEPLOYDIR
			def TEST_LEVEL=env.TEST_LEVEL
			def ALIAS=env.ALIAS
			def DOCKER_SFORG=env.DOCKER_SFORG
			def SF_INSTANCE_URL = env.SF_INSTANCE_URL ?: "https://test.salesforce.com"
			
    }
   

    // -------------------------------------------------------------------------
    // Run all the enclosed stages with access to the Salesforce
    // JWT key credentials.
    // -------------------------------------------------------------------------

 	withEnv(["HOME=${env.WORKSPACE}"]) {	
	
			println "*** SF_USERNAME=" + env.SF_USERNAME
			println "*** server_key_file1=" + SERVER_KEY_CREDENTIALS_ID
			println "*** server_key_file2=" + env.SERVER_KEY_CREDENTIALS_ID
			println "*** toolbelt=" + toolbelt


		stage('SFDX Command help from my SF Docker image') {
			// ERROR: Couldn't connect to Docker daemon at http+docker://localunixsocket - is it running? 
			// Temporary fix but must be avoided: run sudo with the command
			// or refer to https://github.com/docker/compose/issues/6677		
			// Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: 
			// Post http://%2Fvar%2Frun%2Fdocker.sock/v1.29/containers/sfdx-jenkins-org_sforg_1/exec: dial unix /var/run/docker.sock: connect: permission denied
			// Strange thing is that when I run the same command inside the docker instance as jenkins user: the command works fine ??!!
			//command "docker exec sfdx-jenkins-org_sforg_1 sfdx --help"
			command "sudo docker exec sfdx-jenkins-org_sforg_1 sfdx force"
		}

		// ${toolbelt}/
		// -------------------------------------------------------------------------
		// Authenticate to Salesforce using the server key.
		// -------------------------------------------------------------------------

		stage('Authorize to Salesforce - version 1') {
			rc = command "sudo ${toolbelt}/sfdx force:auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${SERVER_KEY_CREDENTIALS_ID} --username ${SF_USERNAME} --setalias ${ALIAS}"
			
			if (rc != 0) {
			error 'Salesforce org authorization failed.'
			} else {
				println "*** Authenticated successfully with " + env.SF_USERNAME
			}
		}


		
		stage('Authorize to Salesforce - version 2 - Running from another container') {
			// ERROR: Couldn't connect to Docker daemon at http+docker://localunixsocket - is it running? 
			// Temporary fix but must be avoided: run sudo with the command
			// or refer to https://github.com/docker/compose/issues/6677		
			// Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: 
			// Post http://%2Fvar%2Frun%2Fdocker.sock/v1.29/containers/sfdx-jenkins-org_sforg_1/exec: dial unix /var/run/docker.sock: connect: permission denied
			// Strange thing is that when I run the same command inside the docker instance as jenkins user: the command works fine ??!!
			//command "docker exec sfdx-jenkins-org_sforg_1 sfdx --help"
			// STILL this error: Ensure the file /var/jenkins_home/workspace/salesforce_demo_org@2/.sfdx/key.json has the file permission octal value of 600. >> add sudo

			rc = command "sudo docker exec ${DOCKER_SFORG} sfdx force:auth:jwt:grant --instanceurl ${SF_INSTANCE_URL} --clientid ${SF_CONSUMER_KEY} --jwtkeyfile ${SERVER_KEY_CREDENTIALS_ID} --username ${SF_USERNAME} --setalias ${ALIAS}"
			
			if (rc != 0) {
			error 'Salesforce org authorization failed.'
			} else {
				println "*** Authenticated successfully with " + env.SF_USERNAME
			}
		}
		
		// -------------------------------------------------------------------------
		// Deploy metadata and execute unit tests.
		// -------------------------------------------------------------------------

		stage('Deploy and Run Tests - V1 from jenkins docker') {
		    rc = command "sudo ${toolbelt}/sfdx force:mdapi:deploy --wait 10 --deploydir ${workspace}/${DEPLOYDIR} --targetusername ${ALIAS} --testlevel ${TEST_LEVEL}"
			if (rc != 0) {
				error 'Salesforce deploy and test run failed.'
		    }
		}

		stage('Deploy and Run Tests - V2 from sforg docker') {
		    rc = command "sudo docker exec ${DOCKER_SFORG} sfdx force:mdapi:deploy --wait 10 --deploydir ${workspace}/${DEPLOYDIR} --targetusername ${ALIAS} --testlevel ${TEST_LEVEL}"
			if (rc != 0) {
				error 'Salesforce deploy and test run failed.'
		    }
		}

		// -------------------------------------------------------------------------
		// Example shows how to run a check-only deploy.
		// -------------------------------------------------------------------------

		stage('Check Only Deploy - V1 from jenkins docker') {
			println ""
		   // Run in the current workspace of Jenkins
		   //rc = command "sudo ${toolbelt}/sfdx force:mdapi:deploy --checkonly --wait 10 --deploydir ${DEPLOYDIR} --targetusername  ${ALIAS} --testlevel ${TEST_LEVEL}"
		   
		   // From another Docker, As i have mounted the volume the path is: /var/jenkins_home/workspace/salesforce_demo_org/src/
		   // So a tweak is necessay to make the call working
		   // TODO: HARDOCDED Value for time beeing
		   rc = command "sudo docker exec ${DOCKER_SFORG} sfdx force:mdapi:deploy --checkonly --wait 10 --deploydir /var/jenkins_home/workspace/salesforce_demo_org/${DEPLOYDIR} --targetusername  ${ALIAS} --testlevel ${TEST_LEVEL}"
		   if (rc != 0) {
		       error 'Salesforce deploy failed.'
		   }
		
		
		stage('Check Only Deploy - V2 from sforg docker') {
			println ""
		   // Run in the current workspace of Jenkins
		   //rc = command "sudo ${toolbelt}/sfdx force:mdapi:deploy --checkonly --wait 10 --deploydir ${DEPLOYDIR} --targetusername  ${ALIAS} --testlevel ${TEST_LEVEL}"
		   
		   // From another Docker, As i have mounted the volume the path is: /var/jenkins_home/workspace/salesforce_demo_org/src/
		   // So a tweak is necessay to make the call working
		   // TODO: HARDOCDED Value for time beeing
		   rc = command "sudo docker exec ${DOCKER_SFORG} sfdx force:mdapi:deploy --checkonly --wait 10 --deploydir  ${workspace}/${DEPLOYDIR} --targetusername  ${ALIAS} --testlevel ${TEST_LEVEL}"
		   if (rc != 0) {
		       error 'Salesforce deploy failed.'
		   }
		}}		


		// TODO: ERROR: Could not find credentials entry with ID '/var/jenkins_home/workspace/salesforce_demo_org/certificates/devhub/server.key'
		// with is matching SERVER_KEY_CREDENTIALS_ID  >>> Guess: must be defined as secret in Jenkins variable
	    //withCredentials([file(credentialsId: SERVER_KEY_CREDENTIALS_ID, variable: 'server_key_file')]) {
			println "*** DO YOU SEE ME - This is the end.***"
	  	//}
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
