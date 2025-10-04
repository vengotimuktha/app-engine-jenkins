pipeline {
  agent any

  environment {
    PROJECT_ID = 'devops-practice-474016'
    // Bind the path of your uploaded Service Account JSON (Credentials → Secret file, ID = gcp-service-account)
    GOOGLE_APPLICATION_CREDENTIALS = credentials('gcp-service-account')
  }

  stages {
    stage('Checkout') {
      steps {
        git branch: 'main',
            url: 'https://github.com/vengotimuktha/app-engine-jenkins.git'
      }
    }

    stage('Deploy to Google App Engine') {
      steps {
        sh 'gcloud --version'
        sh 'pwd && ls -la'       // optional: confirm app.yaml is here

        // Auth & deploy
        sh 'gcloud auth activate-service-account --key-file="$GOOGLE_APPLICATION_CREDENTIALS"'
        sh 'gcloud config set project "$PROJECT_ID"'
        // If app.yaml is not in the repo root, replace app.yaml with its relative path, e.g. ./app/app.yaml
        sh 'gcloud app deploy app.yaml --quiet'
      }
    }
  }

  post {
    always  { cleanWs() }
    success { echo 'Deployment successful!' }
    failure { echo 'Deployment failed!' }
  }
}
