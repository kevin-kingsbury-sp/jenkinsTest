#!/usr/bin/env groovy

pipeline {
    agent any
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
    }
    post {
        always {
            emailext body: '$DEFAULT_CONTENT', replyTo: '$DEFAULT_REPLYTO', subject: '$DEFAULT_SUBJECT', to: 'kevin.kingsbury@sailpoint.com'
        }
    }
}

