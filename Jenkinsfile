pipeline{
	agent {
		label 'master'
	}

    /* properties([buildDiscarder(logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: "${env.BRANCH_NAME}"=="main"?'5':'7', numToKeepStr: '3'))]) */
	stages{
		stage('stage_0:get_tokens') {
			agent {
				label 'master'
			}
			steps{
            			script {
              			sh'''
              			date > $(date '+%d-%m-%Y_%H-%M-%S').txt
              			'''
              			}
			}
		}
		
		stage('Archive Artifacts') {
			agent {
				label 'master'
			}
			steps {
				archiveArtifacts artifacts: '**/*.txt',
				fingerprint: false,
				onlyIfSuccessful: true
				cleanWs()
			}
		}
	}
	post {
        // Clean after build
        cleanup{
        deleteDir()
	}
		always {
			script {
                		def branchName = env.BRANCH_NAME
                		if (branchName == 'develop') {
                    		options {
                        		buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
				}
				} else if (branchName ==~ /release/) {
					options {
                        			buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
					}
				} else {
					options {
                        			buildDiscarder(logRotator(numToKeepStr: '3', artifactNumToKeepStr: '3'))
					}
				}
			}
		}
	}
}
