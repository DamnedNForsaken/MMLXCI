#!/usr/bin/env groovy

void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/DamnedNForsaken/testjenkins"],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

pipeline {
    agent { label 'ubuntu18slave' }
    options { 
        ansiColor('xterm')
        timeout(time: 24, unit: 'HOURS')
    }
    parameters{
	//switch this to deployment key TODO
        string(name: 'mmlxRepoGitUrl', defaultValue: 'https://github.com/DamnedNForsaken/MMLX', description: 'The base repo')
    }
    stages {
        stage('build') {
            steps {
                println("Welcome - You are in the parent container")
                sh 'java -version'
		//dir("/home/ue4/") {
                  //  git credentialsId: 'GH_AUTOMATION_KEY', url: "${mmlxRepoGitUrl}", branch: "${GIT_COMMIT}"
                //}
            }//end build steps
        }//end stage build
	stage('last') {
	    steps {
                sh "echo almost"
                sh "echo done"
	    }//end last steps
	}//end last stage
    }//end stages
    post {
        failure {
            // The build failed
            setBuildStatus("Build Failed", "SUCCESS");
            echo "##### ${JOB_BASE_NAME}, Build ${BUILD_DISPLAY_NAME} was not successful #####"
        }//end post failure
        success {
            // The build was successful
            setBuildStatus("Build Failed", "FAILURE");
            echo "##### ${JOB_BASE_NAME}, Build ${BUILD_DISPLAY_NAME} was successful #####"
        }//end post success
        always {
            println("Welcome - You are in the Parent container (post-always block)")
            echo "##### Cleaning up workspace #####"
            deleteDir()
            echo "##### Finished ${JOB_BASE_NAME}, Build ${BUILD_DISPLAY_NAME}"
        }//end always
    }//end post
}//end pipeline
