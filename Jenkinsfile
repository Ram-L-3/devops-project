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
        stage('Deploy to server') {
            steps {
                sh '''
                    tar czf - app requirements.txt | ssh deploy@fake-server "mkdir -p ~/myapp && tar xzf - -C ~/myapp"
                    ssh deploy@fake-server '
                        cd ~/myapp
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install -q -r requirements.txt
                        if [ -f app.pid ]; then kill $(cat app.pid) || true; fi
                        nohup python app/app.py > app.log 2>&1 < /dev/null &
                        echo $! > app.pid
                    '
                '''
            }
        }
        stage('Verify server') {
            steps {
                sh 'sleep 3 && ssh deploy@fake-server "curl -s localhost:5000/health"'
            }
        }
    }
}
