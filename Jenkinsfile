pipeline {
    agent {
        label 'master'
    }
    
    stages {
        stage('stage_0:get_tokens') {
            steps {
                script {
                    sh'''
                    date > $(date '+%d-%m-%Y_%H-%M-%S').txt
                    '''
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.txt',
                fingerprint: false,
                onlyIfSuccessful: true
                cleanWs()
            }
        }
    }

    post {
        always {
            script {
                def branchName = env.BRANCH_NAME ?: 'unknown'
                if (branchName == 'develop') {
                    deleteArtifacts(15)
                } else if (branchName ==~ /.*release.*/) {
                    deleteArtifacts(10)
                } else {
                    deleteArtifacts(3)
                }
            }
        }
    }
}

def deleteArtifacts(numToKeep) {
    def artifactPattern = "**/*.txt"
    def buildDiscarder = buildDiscarder(logRotator(numToKeepStr: numToKeep.toString(), artifactNumToKeepStr: numToKeep.toString()))
    deleteDir()
}
