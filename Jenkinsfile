pipeline {
  agent { label 'linux' }
  stages {
    stage('build') {
      steps {
        echo 'Building...'
      }
    }

    stage('test') {
      steps {
        echo 'Testing...'
      }
    }

    stage('Promote to RC') {
      input "Promote to Release Candidate Build?"
      milestone()
      node {
        echo 'Promoting to RC'
      }
    }
  }
}

