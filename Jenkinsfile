#!/usr/bin/env groovy

import groovy.transform.Field;

// Pipeline configuration
@Field def buildNode = "r&d"
@Field def buildersEmail = "kevin.kingsbury@sailpoint.com"

def sendBuildEmail(buildStatus) {
    def recipients = buildersEmail

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
            step([$class: 'JUnitResultArchiver', testResults: 'junitreports/*.xml'])
            sendBuildEmail(currentBuild.currentResult)
        }
    }
}
