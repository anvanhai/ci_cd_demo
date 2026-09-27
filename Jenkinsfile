pipeline {
  agent any
  options { disableConcurrentBuilds() }
  triggers { pollSCM('H/2 * * * *') }
  tools { nodejs 'node20' }

  environment {
    VERCEL_TOKEN       = credentials('demo-vercel-token')
    VERCEL_ORG_ID      = credentials('demo-vercel-org-id')
    VERCEL_PROJECT_ID  = credentials('demo-vercel-project-id')
    TELEGRAM_BOT_TOKEN = credentials('demo-telegram-token')
    TELEGRAM_CHAT_ID   = credentials('demo-telegram-chat-id')
    REPO_NAME          = 'anvanhai/ci_cd_demo'
    SITE_URL           = 'https://ci-cd-phi-two.vercel.app'
  }

  stages {
    stage('Thong bao bat dau') {
      steps {
        script {
          env.COMMIT_MSG = sh(script: "git log -1 --pretty=%s", returnStdout: true).trim()
        }
        sh '''
          curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
            -d chat_id=$TELEGRAM_CHAT_ID \
            -d text="Bat dau deploy website
Repository: $REPO_NAME
Branch: main
Commit: $COMMIT_MSG"
        '''
      }
    }

    stage('Deploy Vercel') {
      steps {
        sh 'npx --yes vercel deploy --prod --yes --token $VERCEL_TOKEN'
      }
    }
  }

  post {
    success {
      sh '''
        curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
          -d chat_id=$TELEGRAM_CHAT_ID \
          -d text="Deploy thanh cong
Repository: $REPO_NAME
Branch: main
Website: $SITE_URL"
      '''
    }
    failure {
      sh '''
        curl -s -X POST "https://api.telegram.org/bot$TELEGRAM_BOT_TOKEN/sendMessage" \
          -d chat_id=$TELEGRAM_CHAT_ID \
          -d text="Deploy that bai
Repository: $REPO_NAME
Branch: main
Commit: $COMMIT_MSG
Error: xem chi tiet tai Jenkins Console Output"
      '''
    }
  }
}