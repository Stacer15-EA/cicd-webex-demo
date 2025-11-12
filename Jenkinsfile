pipeline {
    agent any

    stages {
        stage('Setup Python') {
            steps {
                sh 'python3 -m venv venv'
                sh 'source venv/bin/activate && pip install --upgrade pip pytest'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'source venv/bin/activate && pytest'
            }
        }
    }
}
