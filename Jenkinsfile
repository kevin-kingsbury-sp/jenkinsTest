#!/usr/bin/env groovy
def buildNode = "master"

def sendBuildEmail(buildStatus) {
    def recipients = "kevin.kingsbury@sailpoint.com"

    echo "Build status is " + buildStatus

    if (buildStatus == 'FAILURE' || buildStatus == 'UNSTABLE') {
        // Add the Culprits and Suspects to the recipients
        recipients = recipients + " " + emailextrecipients([
            [$class: 'FirstFailingBuildSuspectsRecipientProvider'],
            [$class: 'CulpritsRecipientProvider']
        ])
    }
    echo "Recipients: " + recipients

    emailext subject: '$DEFAULT_SUBJECT',
        body: '$DEFAULT_CONTENT',
        replyTo: '$DEFAULT_REPLYTO',
        to: recipients
}

pipeline {
    agent { label "${buildNode}" }
    options {
        skipDefaultCheckout()
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building...'
            }
        }

        stage('Test') {
            steps {
                echo 'Testing...'
            }
        }
    }
    post {
        always {
            sendBuildEmail(currentBuild.currentResult)
        }
    }
}
