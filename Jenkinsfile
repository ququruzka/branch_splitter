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
                    buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
                } else if (branchName ==~ /.*release.*/) {
                    buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '10'))
                } else {
                    buildDiscarder(logRotator(numToKeepStr: '3', artifactNumToKeepStr: '3'))
                }
                deleteDir()
            }
        }
    }
}
