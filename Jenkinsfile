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
                    docker.image('trufflesecurity/trufflehog')
                        .run("-v ${reportDir}:/reports trufflehog --json ${repoURL} > /reports/report.json")
                    archiveArtifacts artifacts: 'reports/*.json', allowEmptyArchive: true
                }
            }
        }
    }
}
