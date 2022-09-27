pipeline {
  agent any
   stages {
    stage ('Build') {
      steps {
        sh '''#!/bin/bash
        python3 -m venv test3
        source test3/bin/activate
        pip install pip --upgrade
        pip install -r requirements.txt
        export FLASK_APP=application
        flask run &
        '''
     }
      post {
        success {
          slackSend(message: "FYI: ${BUILD_TAG) has SUCCESSFULLY completed its 'BUILD' stage")
        }
        failure {
          slackSend(message: "ATTENTION: ${BUILD_TAG) has FAILED its 'BUILD' stage")
   }
    stage ('test') {
      steps {
        sh '''#!/bin/bash
        source test3/bin/activate
        py.test --verbose --junit-xml test-reports/results.xml
        ''' 
      }
    
      post {
        always {
          junit 'test-reports/results.xml'
        }
        success {
          slackSend(message: "FYI: ${BUILD_TAG) has SUCCESSFULLY completed its 'TEST' stage")
        }
        failure {
          slackSend(message: "ATTENTION: ${BUILD_TAG) has FAILED its 'TEST' stage")
        }
      }
    }
    stage ('Deploy') {
      steps {
        sh '/var/lib/jenkins/.local/bin/eb deploy url-shortener-dev'
     }
      post {
        success {
          slackSend(message: "FYI: ${BUILD_TAG) has SUCCESSFULLY completed its 'DEPLOY' stage")
        }
        failure {
          slackSend(message: "ATTENTION: ${BUILD_TAG) has FAILED its 'DEPLOY' stage")
        }
    } 
  }
 }
