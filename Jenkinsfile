pipeline {
    agent any

    environment {
        BUILD_VERSION = "v${BUILD_NUMBER}"
    }

    stages {

        stage('Build') {
            steps {
                echo "Building artifact version ${BUILD_VERSION}"
                sh '''
                  echo "This is build ${BUILD_NUMBER}" > app.txt
                  zip app-${BUILD_VERSION}.zip app.txt
                '''
            }
        }

        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: 'app-*.zip', fingerprint: true
            }
        }
    }
}
