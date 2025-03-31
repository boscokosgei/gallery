pipeline {
    agent any

    tools {
        nodejs 'NodeJS 23.10.0'
    }

    environment {
        DEPLOY_HOOK_URL = 'https://api.render.com/deploy/srv-cvkodk49c44c73d8hqg0?key=3xveVvjJDvg'
    }

    stages {
        stage('Node Version') {
            steps {
                echo 'Checking Node.js version...'
                sh 'node --version'
            }
        }

        stage('Clone repo') {
            steps {
                echo 'Cloning the repository...'
                git credentialsId: 'gitconnect', url: 'https://github.com/boscokosgei/gallery.git'
            }
        }

        stage('Install Npm') {
            steps {
                echo 'Installing npm packages...'
                sh 'npm install'
                sh 'npm install mongodb'
                sh 'npm install -g webpack'
            }
        }

        stage('Build') {
            steps {
                echo 'Running the build...'
                sh 'npm run build'
            }
        }

        stage('Test') {
            when {
                expression { return false } // 🔧 Disable for now
            }
            steps {
                echo 'Skipping tests...'
            }
        }

        stage('Deploying to Render122') {
            steps {
                script {
                    def response = sh(script: """
                        curl -X POST ${DEPLOY_HOOK_URL}
                    """, returnStdout: true).trim()
                   
                    echo "Deploying Responses: ${response}"
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline complete.'
        }

        success {
            slackSend(
                botUser: true,
                channel: 'C08L0RA7KRQ',
                color: '#36a64f',  
                message: "✅ Your Deployment has been  successful! Build ID - ${env.BUILD_ID}. 🚀 Check it here: https://gallery-8f5e.onrender.com",
                teamDomain: 'boscoip1',
                tokenCredentialId: 'slacklog'
            )
        }

        failure {
            echo 'Pipeline failed.'
        }
    }
}
