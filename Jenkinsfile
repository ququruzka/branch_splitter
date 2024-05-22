def branchName = "${env.BRANCH_NAME}"

pipeline {
    agent {
        label 'master'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: (branchName != 'develop' || branchName ==~ /.*release.*/) ? '3' : '6'))
    }

    stages {
        stage('stage_0:get_tokens') {
            steps {
                script {
                    sh'''
                    mkdir -p ./openwrt.mw.debug/bin/targets/ipq53xx/ipq53xx_32/
                    date > openwrt.mw.debug/bin/targets/ipq53xx/ipq53xx_32/$(date '+%d-%m-%Y_%H-%M-%S').bin
                    '''
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/*.bin',
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
