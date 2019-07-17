def sendNotification(String buildStatus = 'STARTED') {
    echo "MESSAGE: buildStatus is ${buildStatus}"
    emailext attachLog: true,
        body: '$DEFAULT_CONTENT',
        subject: '$DEFAULT_SUBJECT',
        to: 'kevin.kingsbury@sailpoint.com'
}

def getChangeString() {
    def changeString = ""

    echo "Gathering SCM changes"
    def changeLogSets = currentBuild.changeSets
    for (int i = 0; i < changeLogSets.size(); i++) {
      def entries = changeLogSets[i].items
      for (int j = 0; j < entries.length; j++) {
        def entry = entries[j]
        echo "${entry.commitId} by ${entry.author} on ${new Date(entry.timestamp)}: ${entry.msg}"
        def files = new ArrayList(entry.affectedFiles)
          for (int k = 0; k < files.size(); k++) {
            def file = files[k]
            echo "  ${file.editType.name} ${file.path}"
          }
      }
    }

    if (!changeString) {
        changeString = " - No new changes"
    }
    return changeString
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
                    getChangeString
                }
            }
        }
        stage('Build') {
            steps {
                node('sunfish') {
                    sh 'echo "Running build!!!"; printenv'
                    sh 'export revisionts=20190716114825;echo "Running build2!!!"; printenv'
                }
            }
        }
        stage('Test') {
            steps {
                node('sunfish') {
                    sh 'echo "Success!"; printenv; exit 0'
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
