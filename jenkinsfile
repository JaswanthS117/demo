pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/JaswanthS117/demo.git'  // Update with your ColdFusion app repository
        CF_DEST_PATH = 'C:\inetpub\wwwroot\qr-api'  // Update to your ColdFusion app's deployment directory
        GIT_CREDENTIALS_ID = '7be70f43-5ba8-4225-bf84-7ad5946b593a'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: "${GIT_CREDENTIALS_ID}", url: "${GIT_REPO}", branch: 'main'
            }
        }
        stage('Prepare Deployment Directory') {
            steps {
                script {
                    echo "Cleaning old files in ${CF_DEST_PATH}"
                    bat """
                        powershell -command "if (Test-Path '${CF_DEST_PATH}') { Remove-Item -Recurse -Force '${CF_DEST_PATH}' }"
                        powershell -command "New-Item -ItemType Directory -Path '${CF_DEST_PATH}'"
                    """
                }
            }
        }
        stage('Copy Files to Server') {
            steps {
                script {
                    echo "Copying application files to ${CF_DEST_PATH}"
                    bat """
                        xcopy "${WORKSPACE}\*" "${CF_DEST_PATH}" /E /H /Y /C
                    """
                }
            }
        }
    }
    post {
        success {
            echo 'ColdFusion application deployment successful!'
        }
        failure {
            echo 'ColdFusion application deployment failed.'
        }
    }
}
