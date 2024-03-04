pipeline {
    agent any
    triggers {
        cron('0 5 * * *')
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '7'))
    }
    stages {
        stage('Initialize') {
            steps {
                echo "Workspace: ${env.WORKSPACE}"
                echo "Job Name: ${env.JOB_NAME}"
                echo "User: ${env.USER}"
                echo "Machine Name: ${env.COMPUTERNAME}"
                deleteDir()
            }
        }
        stage('Scan with TruffleHog') {
            steps {
                script {
                    def reportDir = "${env.WORKSPACE}/reports"
                    def repoURL = 'https://github.com/johnlaurance/TP_SECDEVOPS.git'
                    // Create reports directory if it doesn't exist
                    sh "mkdir -p ${reportDir}"
                    // Run TruffleHog and save report to the reports directory
                    docker.image('trufflesecurity/trufflehog')
                        .run("-v ${reportDir}:/reports trufflehog --json ${repoURL} > ${reportDir}/report.json")
                    // Archive TruffleHog report as artifact
                    archiveArtifacts artifacts: 'reports/*.json', allowEmptyArchive: true
                }
            }
        }
    }
}
