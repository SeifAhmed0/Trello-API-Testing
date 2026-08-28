pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SeifAhmed0/Trello-API-Testing.git'
            }
        }
        stage('Verify Newman') {
            steps {
                sh 'newman --version'
            }
        }
        stage('Run Postman Tests') {
            steps {
                withCredentials([file(credentialsId: 'trello-env-file', variable: 'ENV_FILE')]) {
                    sh '''
                        newman run "Trello API.postman_collection.json" -e "$ENV_FILE" -r cli,htmlextra --reporter-htmlextra-export reports/report.html
                    '''
                }
            }
        }
    }
    post {
        always {
            publishHTML(target: [
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'reports',
                reportFiles: 'report.html',
                reportName: 'Newman HTML Report'
            ])
            archiveArtifacts artifacts: 'reports/report.html', allowEmptyArchive: true
        }
    }
}
