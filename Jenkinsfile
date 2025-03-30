pipeline {
    agent any
    tools {
        nodejs 'NodeJS 23.10.0' // Ensure this name matches your Global Tool Configuration
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
        stage('Cloning repository') {
            steps {
                echo 'Cloning  the Gallery repository...'
                git credentialsId: 'gitconnect', url: 'https://github.com/boscokosgei/gallery.git'
            }
        }
        stage('Install Npm Dependencies') {
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
            steps {
                echo 'Running this tests...'
                sh 'npm test'
            }
        }
        stage('Deploying to Render122') {
                        steps {
                            script {
                                def response = sh(script: """
                                    curl -X POST ${DEPLOY_HOOK_URL}
                                """, returnStdout: true).trim()
                                
                                echo "Deploying Response: ${response}"
                            }
                        }

                    }
                    stage('send message to slack'){
                        steps{
                            slackSend(
                    botUser: true, 
                    channel: 'C08L0RA7KRQ', 
                    color: '',  
                    message: "Deployment successful! Build ID - ${env.BUILD_ID}. Check the deployed site: https://gallery-8f5e.onrender.com", 
                    teamDomain: 'Bosco_IP1', 
                    tokenCredentialId: 'slacklog'
                )

                        }
                    }
                    
    }

    post {
        always {
            echo 'Pipeline completed.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
