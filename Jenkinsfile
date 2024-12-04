pipeline {
    agent any

    environment {
        CI = 'true'
    }

    stages {
        stage('Build') {
            steps {
                bat 'npm install'
            }
        }

        stage('Test') {
            steps {
                // Ensure script is executable and then run it
                bat '"C:\\Program Files\\Git\\bin\\bash.exe" -c "chmod +x ./jenkins/scripts/test.sh && ./jenkins/scripts/test.sh"'
            }
        }
    }
}