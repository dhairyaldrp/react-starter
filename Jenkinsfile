pipeline {
    agent any
    tools {
        nodejs "node"
    }
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
                SERVER_CONFIG_FILE = 'server-config.json'
                SERVER_CONFIG = sh(returnStdout: true, script: "cat ${SERVER_CONFIG_FILE} | jq -r '.serverIP, .serverUsername, .serverPassword, .remotePath'")

                // Customize these variables with your Windows Server details
                // WINDOWS_SERVER = '35.176.66.224'
                // WINDOWS_USERNAME = 'Administrator'
                // WINDOWS_PASSWORD = 'Gujarat1@'
                // REMOTE_PATH = 'C:/react' // Change this path to your website directory on the Windows Server
            }

            steps {
                script {
                    def (serverIP, serverUsername, serverPassword, remotePath) = SERVER_CONFIG.tokenize()
                    sh "sshpass -p '${serverPassword}' scp -o StrictHostKeyChecking=no -r build/* ${serverUsername}@${serverIP}:${remotePath}"

                }
            }
            // steps {
            //     // Copy build artifact to Windows Server using SCP (Assuming you have SSH access)
            //     sh "sshpass -p ${WINDOWS_PASSWORD} scp -o StrictHostKeyChecking=no -r build/* ${WINDOWS_USERNAME}@${WINDOWS_SERVER}:${REMOTE_PATH}"

            //     // Restart IIS service on the Windows Server
            //     // Note: This assumes you have appropriate permissions to restart the service
            //     //sh "sshpass -p ${WINDOWS_PASSWORD} ssh -o StrictHostKeyChecking=no ${WINDOWS_USERNAME}@${WINDOWS_SERVER} 'powershell Stop-Website -Name \"react\"; Start-Website -Name \"react\"'"
            // }
        }
    }
}
