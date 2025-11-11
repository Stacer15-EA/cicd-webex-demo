pipeline {
  agent any

  environment {
    WEBEX_BOT_TOKEN = credentials('WEBEX_BOT_TOKEN') // stored in Jenkins credentials
    WEBEX_ROOM_ID = credentials('WEBEX_ROOM_ID')
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }

    stage('Install dependencies') {
      steps {
        sh 'python3 -m venv venv || true'
        sh '. venv/bin/activate && pip install -r requirements.txt'
      }
    }

    stage('Run tests') {
      steps {
        sh '. venv/bin/activate && pytest -q'
      }
    }
  }

  post {
    success {
      script {
        def msg = "✅ Build SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|open>)"
        sh """
          curl -s -X POST "https://webexapis.com/v1/messages" \
          -H "Authorization: Bearer ${WEBEX_BOT_TOKEN}" \
          -H "Content-Type: application/json" \
          -d '{"roomId":"${WEBEX_ROOM_ID}","markdown":"${msg}"}'
        """
      }
    }

    failure {
      script {
        def msg = "❌ Build FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER} (<${env.BUILD_URL}|open>)"
        sh """
          curl -s -X POST "https://webexapis.com/v1/messages" \
          -H "Authorization: Bearer ${WEBEX_BOT_TOKEN}" \
          -H "Content-Type: application/json" \
          -d '{"roomId":"${WEBEX_ROOM_ID}","markdown":"${msg}"}'
        """
      }
    }
  }
}
