pipeline {
  agent any
  stages {
    stage('install') {
      steps {
        script {
          npm install
        }

      }
    }

    stage('build') {
      steps {
        echo 'build...'
        script {
          npm run build-dev
        }

      }
    }

    stage('Deploy') {
      steps {
        echo 'Deploying...'
      }
    }

  }
}