pipeline{
	agent {
		label 'master'
	}






  strategy {
                  namedBranchesDifferent {
                    defaultProperties {
                      buildRetention {
                        buildDiscarder {
                          logRotator {
                            daysToKeepStr('184')
                            numToKeepStr('9')
                            artifactDaysToKeepStr('184')
                            artifactNumToKeepStr('9')
                          }
                        }
                      }
                    }
                    namedExceptions {

                      named {
                        name('devel,develop,development,release')
                        props {
                          buildRetention {
                            buildDiscarder {
                              logRotator {
                                daysToKeepStr('10')
                                numToKeepStr('3')
                                artifactDaysToKeepStr('10')
                                artifactNumToKeepStr('3')
                              }
                            }
                          }

                        }
                      }

                  }
                }
}

/*  options {
		timestamps ()
		buildDiscarder(
                        logRotator(
                                artifactDaysToKeepStr: "",
                                artifactNumToKeepStr: "${env.BRANCH_NAME}"=="main"?'5':'3',
                                daysToKeepStr: "",
                                numToKeepStr: "${env.BRANCH_NAME}"=="main"?'5':'3')
              )
    } */
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
    }
}

