pipeline {
  agent any
  options { disableConcurrentBuilds() }
  triggers { pollSCM('H/2 * * * *') }
  tools { nodejs 'node20' }

  environment {
    VERCEL_TOKEN      = credentials('demo-vercel-token')
    VERCEL_ORG_ID     = credentials('demo-vercel-org-id')
    VERCEL_PROJECT_ID = credentials('demo-vercel-project-id')
  }

  stages {
    stage('Kiem tra commit') {
      steps {
        sh 'git log -1 --oneline'
      }
    }
    stage('Deploy Vercel') {
      steps {
        sh 'npx --yes vercel deploy --prod --yes --token $VERCEL_TOKEN'
      }
    }
  }
}