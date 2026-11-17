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
        stage('Deploy') {
            steps {
                sh 'docker rm -f devops-app || true'
                sh 'docker run -d --name devops-app -p 5001:5000 devops-project:jenkins-${BUILD_NUMBER}'
            }
        }
        stage('Health check') {
            steps {
                sh """
                sleep 3
                docker exec devops-app python -c "import urllib.request as u; print(u.urlopen('http://localhost:5000/health').read())"
                """
            }
        }
    }
}
