pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                script {
                    cleanWs()
                    git credentialsId: 'github-token', url: 'https://github.com/bawdsr/abcd-student', branch: 'main'
                }
            }
        }
        stage('ZAP Scan') {
            steps {
                sh '''
                    docker run --name zap \
                    -v "${WORKSPACE}/abcd-student/.zap:/zap/wrk:rw" \
                    -v "${WORKSPACE}/reports:/zap/wrk/reports" \
                    -t ghcr.io/zaproxy/zaproxy:stable bash -c \\
                    "zap.sh -cmd -addonupdate && \\
                    zap.sh -cmd -addoninstall communityScripts && \\
                    zap.sh -cmd -addoninstall pscanrulesAlpha && \\
                    zap.sh -cmd -addoninstall pscanrulesBeta && \\
                    zap.sh -cmd -autorun /zap/wrk/passive.yaml"
                '''
            }
        }
    }
}