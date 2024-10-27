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
        // stage('ZAP Scan') {
        //     steps {
        //         sh '''
        //         docker run --name juice-shop -d --rm \
        //             -p 3000:3000 bkimminich/juice-shop
        //         sleep 5
        //         '''
        //         sh '''
        //             docker run --name zap \
        //             -v /mnt/c/Users/baw.DSR/ABCD/abcd-student/.zap:/zap/wrk:rw \
        //             -v /mnt/c/Users/baw.DSR/ABCD/reports:/zap/wrk/reports \
        //             -t ghcr.io/zaproxy/zaproxy:stable bash -c \\
        //             "zap.sh -cmd -addonupdate && \\
        //             zap.sh -cmd -addoninstall communityScripts && \\
        //             zap.sh -cmd -addoninstall pscanrulesAlpha && \\
        //             zap.sh -cmd -addoninstall pscanrulesBeta && \\
        //             zap.sh -cmd -autorun /zap/wrk/passive.yaml"
        //         '''
        //     }
        // }
        stage('OSV-scanner Scan') {
            steps {
                sh '''
                    osv-scanner --lockfile package-lock.json --format json --output osvscanner-report.json
                '''
            }
        }
    }   
    post {
        always {
            // sh '''
            //     mkdir -p ${WORKSPACE}/results/
            //     docker cp zap:/zap/wrk/reports/zap_html_report.html ${WORKSPACE}/results/zap_html_report.html
            //     docker cp zap:/zap/wrk/reports/zap_xml_report.xml ${WORKSPACE}/results/zap_xml_report.xml
            //     docker stop zap juice-shop
            //     docker rm zap
            //     '''
            // defectDojoPublisher(
            //     artifact: '${WORKSPACE}/results/zap_xml_report.xml', 
            //     productName: 'Juice Shop', 
            //     scanType: 'ZAP Scan', 
            //     engagementName: 'bartosz.wlazlo@dsr.com.pl'
            // )
            // defectDojoPublisher(
            //     artifact: '${WORKSPACE}/osvscanner-report.json', 
            //     productName: 'Juice Shop', 
            //     scanType: 'OSV Scan', 
            //     engagementName: 'bartosz.wlazlo@dsr.com.pl'
            // )
        }
    }
}