pipeline {
    agent any

    parameters {
        string(name: 'ROLLBACK_BUILD',
               defaultValue: '',
               description: 'Build number to rollback (leave empty for normal build)')
    }

    stages {

        stage('Build') {
            when {
                expression { params.ROLLBACK_BUILD == '' }
            }
            steps {
                echo "Building new version"
                bat '''
                mkdir dist
                echo version %BUILD_NUMBER% > dist\\app.txt
                tar -a -c -f app-v%BUILD_NUMBER%.zip dist
                '''
            }
        }

        stage('Archive Artifact') {
            when {
                expression { params.ROLLBACK_BUILD == '' }
            }
            steps {
                archiveArtifacts artifacts: '*.zip', fingerprint: true
            }
        }

        stage('Rollback') {
            when {
                expression { params.ROLLBACK_BUILD != '' }
            }
            steps {
                echo "Rolling back to build #${params.ROLLBACK_BUILD}"
                copyArtifacts(
                    projectName: env.JOB_NAME,
                    selector: specific(params.ROLLBACK_BUILD)
                )
                echo "Rollback artifact fetched successfully"
            }
        }
    }
}
