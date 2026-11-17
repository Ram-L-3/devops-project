pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('Checkout') {
            steps { checkout scm }
        }
        stage('Install') {
            steps {
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements-dev.txt'
            }
        }
        stage('Test') {
            steps {
                sh '. venv/bin/activate && python -m pytest'
            }
        }
        stage('Build image') {
            steps {
                sh 'docker build -t devops-project:jenkins-${BUILD_NUMBER} .'
            }
        }
    }
}
