pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/YOUR-USERNAME/YOUR-REPO.git'
            }
        }

        stage('Build') {
            steps {
                sh 'echo "Building the website..."'
                sh 'npm install' // Modify based on your tech stack
                sh 'npm run build' // Modify for your project
            }
        }

        stage('Test') {
            steps {
                sh 'echo "Running tests..."'
                sh 'npm test' // Modify for your testing framework
            }
        }

        stage('Deploy') {
            steps {
                sshagent(['your-ssh-key']) {
                    sh '''
                    ssh user@your-server "cd /var/www/your-website && git pull origin main && npm install && pm2 restart your-app"
                    '''
                }
            }
        }
    }

    post {
        failure {
            script {
                def message = "🚨 Build Failed! Check Jenkins: ${env.BUILD_URL}"
                sh "curl -H 'Content-Type: application/json' -d '{\"text\": \"$message\"}' $TEAMS_WEBHOOK_URL"
            }
        }
    }
}

