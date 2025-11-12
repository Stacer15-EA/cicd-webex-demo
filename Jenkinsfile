pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Pull code from GitHub
                checkout scm
            }
        }

        stage('Setup Python Environment') {
            steps {
                // Set up and install pytest
                sh '''
                python -m venv venv
                source venv/bin/activate
                pip install --upgrade pip pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                // Run your Python unit tests
                sh '''
                source venv/bin/activate
                pytest --maxfail=1 --disable-warnings -q
                '''
            }
        }
    }

    post {
        always {
            script {
                // Prepare the WebEx message
                def message = [
                    roomId: 'Y2lzY29zcGFyazovL3VybjpURUFNOnVzLXdlc3QtMl9yL1JPT00vZjQwNTQ5MjAtYmY2My0xMWYwLTgyNmMtYmIyN2IwZjdiOTM2',
                    markdown: "✅ Jenkins build for *${env.JOB_NAME}* #${env.BUILD_NUMBER} completed with status: *${currentBuild.currentResult}*"
                ]

                // Send message to WebEx
                httpRequest(
                    httpMode: 'POST',
                    contentType: 'APPLICATION_JSON',
                    url: 'https://webexapis.com/v1/messages',
                    customHeaders: [[name: 'Authorization', value: 'Bearer ZmMxYWY5M2EtNzZkNS00Yjk3LTgwNjAtMmM5MzlmNGI4OGQzYzc5NjY2YjMtMTEz_P0A1_13494cac-24b4-4f89-8247-193cc92a7636']],
                    requestBody: groovy.json.JsonOutput.toJson(message)
                )
            }
        }
    }
}
