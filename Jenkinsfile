def sendNotification(String buildStatus = 'STARTED') {
    echo "MESSAGE: buildStatus is ${buildStatus}"
    emailext attachLog: true,
        body: '$DEFAULT_CONTENT',
        subject: '$DEFAULT_SUBJECT',
        to: 'kevin.kingsbury@sailpoint.com'
}

pipeline {
    agent none
    options {
        skipDefaultCheckout()
        buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '', numToKeepStr: '')
    }

    stages {
        stage('Checkout') {
            steps {
                node('sunfish') {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                node('sunfish') {
                    sh 'echo "Running build!!!"; env'
                    env.revisionts = "20190716114825"
                }
            }
        }
        stage('Test') {
            steps {
                node('sunfish') {
                    sh 'echo "Success!"; exit 0'
                }
            }
        }
    }
    post {
        always {
            echo 'This will always run'
            echo "Build Variables are: ${currentBuild.buildVariables}"
            echo "Build Revision Timestamp: ${env.revisionts}"
            sendNotification(currentBuild.currentResult)
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failure'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}
