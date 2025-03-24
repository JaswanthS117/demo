pipeline {
    agent any
    environment {
        GIT_REPO = 'https://github.com/JaswanthS117/demo.git'  // Update with your ColdFusion app repository
        CF_DEST_PATH = 'C:\\inetpub\\wwwroot\\Demo'  // Update to your ColdFusion app's deployment directory
        GIT_CREDENTIALS_ID = '7be70f43-5ba8-4225-bf84-7ad5946b593a'
    }
    stages {
        stage('Clone Repository') {
            steps {
                git credentialsId: "${GIT_CREDENTIALS_ID}", url: "${GIT_REPO}", branch: 'main'
            }
        }
        stage('Deploy Updated Files') {
            steps {
                script {
                    echo "Deploying only updated or new files to ${CF_DEST_PATH}"
                    bat """
                        robocopy "C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\demo" "C:\\inetpub\\wwwroot\\Demo" /E /XD .git /XC /XN /XO
                    """
                }
            }
        }
    }
    post {
        success {
            archiveArtifacts artifacts: '**/*', fingerprint: true
            echo 'ColdFusion application deployment successful!'
        }
        failure {
            echo 'ColdFusion application deployment failed.'
        }
    }
}
