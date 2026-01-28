pipeline {
  agent any

  stages {

    /* -------------------- COMMON STAGES (ALL BRANCHES) -------------------- */

    stage('Checkout') {
      steps {
        echo "Checking out branch: ${env.BRANCH_NAME}"
        checkout scm
      }
    }

    stage('Build') {
      steps {
        sh 'echo "mvn clean compile"'
      }
    }

    stage('Unit Tests') {
      steps {
        sh 'echo "mvn test"'
      }
    }

    stage('Static Scan') {
      steps {
        sh 'echo "Run Sonar / SAST here"'
        // sh 'sonar-scanner'
      }
    }

    /* -------------------- DEV ONLY -------------------- */

    stage('Deploy to DEV') {
      when {
        branch 'dev'
      }
      steps {
        sh 'echo "kubectl apply -f k8s/dev/"'
      }
    }

    stage('Smoke Test - DEV') {
      when {
        branch 'dev'
      }
      steps {
        sh 'echo "curl -f http://dev.myapp.com/health"'
      }
    }

    /* -------------------- PROD ONLY -------------------- */

    stage('Manual Approval') {
      when {
        branch 'main'
      }
      steps {
        input "Deploy to PROD?"
      }
    }

    stage('Deploy to PROD') {
      when {
        branch 'main'
      }
      steps {
        sh 'echo "Deploying to PROD"'
      }
    }
  }

  post {
    success {
      echo "✅ Pipeline completed successfully for ${env.BRANCH_NAME}"
    }
    failure {
      echo "❌ Pipeline failed for ${env.BRANCH_NAME}"
    }
  }
}
