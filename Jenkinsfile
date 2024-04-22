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
                def branchName = env.BRANCH_NAME
                if (branchName == 'develop') {
                    currentBuild.rawBuild.getLogRotator().setNumToKeep(15)
                    currentBuild.rawBuild.getLogRotator().setArtifactNumToKeep(15)
                } else if (branchName ==~ /.*release.*/) {
                    currentBuild.rawBuild.getLogRotator().setNumToKeep(10)
                    currentBuild.rawBuild.getLogRotator().setArtifactNumToKeep(10)
                } else {
                    currentBuild.rawBuild.getLogRotator().setNumToKeep(3)
                    currentBuild.rawBuild.getLogRotator().setArtifactNumToKeep(3)
                }
            }
            script {
                deleteDir()
            }
        }
    }
}
