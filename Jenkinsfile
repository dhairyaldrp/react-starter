pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the source code from Git repository
                git branch: 'master', url: 'https://github.com/dhairyaldrp/react-starter'
            }
        }

        stage('Build') {
            steps {
                // Install Node.js dependencies
                sh 'npm install'

                // Create a production build
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            environment {
                // Customize these variables with your Windows Server details
                WINDOWS_SERVER = 'your_windows_server_ip'
                WINDOWS_USERNAME = 'your_windows_username'
                WINDOWS_PASSWORD = 'your_windows_password'
                REMOTE_PATH = 'C:/path/to/website/directory' // Change this path to your website directory on the Windows Server
            }
            steps {
                // Copy build artifact to Windows Server using SCP (Assuming you have SSH access)
                sh "scp -r build ${WINDOWS_USERNAME}@${WINDOWS_SERVER}:${REMOTE_PATH}"

                // Restart IIS service on the Windows Server
                // Note: This assumes you have appropriate permissions to restart the service
                sh "ssh ${WINDOWS_USERNAME}@${WINDOWS_SERVER} 'iisreset'"
            }
        }
    }
}
