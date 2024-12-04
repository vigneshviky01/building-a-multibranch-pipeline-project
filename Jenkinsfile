pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                echo "Installing dependencies..."
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests..."
                bat(script: '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/test.sh', returnStatus: true)
            }
        }

        stage('Deliver for development') {
            when {
                branch 'development'
            }
            steps {
                echo "Delivering for development on branch: ${env.BRANCH_NAME}"
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deliver-for-development.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                timeout(time: 5, unit: 'MINUTES') {
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
                }
            }
            post {
                success {
                    echo "Development delivery succeeded."
                }
                failure {
                    echo "Development delivery failed."
                }
            }
        }

        stage('Deploy for production') {
            when {
                branch 'production'
            }
            steps {
                echo "Deploying for production on branch: ${env.BRANCH_NAME}"
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/deploy-for-production.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)' 
                timeout(time: 5, unit: 'MINUTES') {
                    bat '"C:\\Program Files\\Git\\bin\\bash.exe" ./jenkins/scripts/kill.sh'
                }
            }
            post {
                success {
                    echo "Production deployment succeeded."
                }
                failure {
                    echo "Production deployment failed."
                }
            }
        }
    }

    post {
        always {
            echo "Cleaning up after pipeline execution..."
        }
    }
}