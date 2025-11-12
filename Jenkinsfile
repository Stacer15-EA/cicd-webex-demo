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

    post {
        always {
            script {
                httpRequest httpMode: 'POST',
                    url: 'https://webexapis.com/v1/messages',
                    contentType: 'APPLICATION_JSON',
                    requestBody: '{"roomId":"Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vZjQwNTQ5MjAtYmY2My0xMWYwLTgyNmMtYmIyN2IwZjdiOTM2","text":"Build finished"}',
                    customHeaders: [[name: 'Authorization', value: 'Bearer ZmMxYWY5M2EtNzZkNS00Yjk3LTgwNjAtMmM5MzlmNGI4OGQzYzc5NjY2YjMtMTEz_P0A1_13494cac-24b4-4f89-8247-193cc92a7636']]
            }
        }
    }
}
