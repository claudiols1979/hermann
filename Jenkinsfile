pipeline {
    agent any 
    options {
        buildDiscarder(LogRotator(numToKeepStr: '10'))
    }

    stages {
        stage ('Build') {
            steps {
                sh 'bundle install'

                sh 'bundle exec rake build spec'

                archive includes: 'pkg/*.gem'


                publishHTML target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: false,
                    keepAll: true,
                    reportDir: 'coverage',
                    reportFiles: 'index.html',
                    reportName: 'RCov Report'

                ]
            }
        }
    }
    post {
        // Clean after build
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true,
                    patterns: [[pattern: '.gitignore', type: 'INCLUDE'],
                               [pattern: '.propsfile', type: 'EXCLUDE']])
        }
    }
}