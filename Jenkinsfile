pipeline {
    agent any

    environment {
        BUILD_VERSION = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Build') {
            steps {
                echo "Building artifact version ${BUILD_VERSION}"
                bat """
                echo This is build %BUILD_NUMBER% > app.txt
                powershell Compress-Archive app.txt app-%BUILD_VERSION%.zip
                """
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'app-*.zip', fingerprint: true
            }
        }
    }
}
