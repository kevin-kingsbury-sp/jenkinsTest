pipeline {
  stages {
    docker.withServer('tcp://192.168.99.100:2376','7f3d7a91-4c77-4601-b70f-2c17e5c4f97f') {
      docker.image('jenkins_slave') {
        stage('build') {
          steps {
            sh 'ant --version'
          }
        }
      }
    }
  }
}

