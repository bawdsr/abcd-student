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
        stage('DAST') {
            steps {
                sh '''
                docker run --name juice-shop -d --rm \
                    -p 3000:3000 bkimminich/juice-shop
                sleep 5
                '''
                sh '''
                docker run --name zap \
                -v C:/Users/baw.DSR/ABCD/abcd-student/:/zap/wrk/:rw \\
                -v C:/Users/baw.DSR/ABCD/Reports/:/zap/wrk/reports \\
                -t ghcr.io/zaproxy/zaproxy:stable bash -c \\
                    "zap.sh -cmd -addonupdate && \\
                    zap.sh -cmd -addoninstall communityScripts && \\
                    zap.sh -cmd -addoninstall pscanrulesAlpha && \\
                    zap.sh -cmd -addoninstall pscanrulesBeta && \\
                    zap.sh -cmd -autorun /zap/wrk/passive_scan.yaml"
                '''
            }
        }
    }
}