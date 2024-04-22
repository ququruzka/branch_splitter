pipeline {
    agent {
        label 'master'
    }

    environment {
        NUM_TO_KEEP = branchName ==~ /^(?!develop$|release)/ ? '5' : '10'
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
            deleteArtifacts(NUM_TO_KEEP)
        }
    }
}

def deleteArtifacts(numToKeep) {
    def artifactPattern = '**/*.txt'
    def buildDiscarderConfig = numToKeep == '10' ? '10' : [numToKeepStr: numToKeep]

    options {
        buildDiscarder(logRotator(buildDiscarderConfig))
    }

    deleteDir()
}
