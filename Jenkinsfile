pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('Code checkout from GitHub') {
            steps {
                cleanWs()
                git credentialsId: 'github-token', url: 'https://github.com/bawdsr/abcd-student', branch: 'main'
            }
        }
        stage('DAST') {
            steps {
                script {
                    def workspace = pwd()
                    sh '''
                    docker rm -f juice-shop || true
                    docker run --name juice-shop -d --rm \
                        -p 3000:3000 bkimminich/juice-shop
                    '''
                    // Wait for the service to be ready
                    sh 'until curl -s http://localhost:3000; do sleep 5; done'
                    sh """
                    docker run --name zap \\
                    -v '${workspace}/':/zap/wrk/:rw \\
                    -t ghcr.io/zaproxy/zaproxy:stable bash -c \\
                        "zap.sh -cmd -addonupdate && \\
                        zap.sh -cmd -addoninstall communityScripts && \\
                        zap.sh -cmd -addoninstall pscanrulesAlpha && \\
                        zap.sh -cmd -addoninstall pscanrulesBeta && \\
                        zap.sh -cmd -autorun /zap/wrk/passive_scan.yaml"
                    """
                }
            }
        }
    }
    post {
        always {
            // Cleanup Docker containers
            sh 'docker rm -f juice-shop zap || true'
        }
    }
}