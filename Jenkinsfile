#!/usr/bin/env groovy

pipeline {
    agent none
    options {
        skipDefaultCheckout()
    }
    stages {
        stage('Checkout') {
            steps {
                node('master') {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                node('master') {
                    echo 'Building...'
                    sh 'false'
                }
            }
        }

        stage('Test') {
            steps {
                node('master') {
                    echo 'Testing...'
                }
            }
        }
    }
    post {
        always {
            node('master') {
                emailext body: '$DEFAULT_CONTENT', replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT', to: 'kevin.kingsbury@sailpoint.com'
            }
        }
        failure {
            node('master') {
                emailext body: '$DEFAULT_CONTENT', recipientProviders: [[$class: 'CulpritsRecipientProvider'], [$class: 'FirstFailingBuildSuspectsRecipientProvider']], replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT'
            }
        }
    }
}
