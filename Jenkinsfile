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
                echo "Branch name: ${branchName}"
                
                if (branchName == 'develop') {
                    echo "Branch name matched 'develop'"
                    buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
                } else if (branchName ==~ /.*release.*/) {
                    echo "Branch name matched 'release' regular expression"
                    buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
                } else {
                    echo "Branch name did not match any condition"
                    buildDiscarder(logRotator(numToKeepStr: '3', artifactNumToKeepStr: '3'))
                }
                
                deleteDir()
            }
        }
    }
}
