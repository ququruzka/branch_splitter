pipeline {
    agent {
        label 'master'
    }

    environment {
        NUM_TO_KEEP = branchName ==~ /^(?!develop$|release)/ ? '3' : '6'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: NUM_TO_KEEP))
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
            deleteDir()
        }
    }
}
