pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Inspect') {
            steps {
                sh 'whoami'
                sh 'ls -la'
                sh 'cat requirements.txt'
            }
        }
        stage('Python check') {
            steps { sh 'python3 --version' }
        }
    }
}
