pipeline {
    agent any

//    triggers {
//        pollSCM('* * * * *')
//    }
    options {
        skipDefaultCheckout(true)
        // Keep the 5 most recent builds
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
    }
    // environment {
     
  //  }

    stages {

        stage ("Code pull"){
            steps{
                checkout scm
            }
        }
        stage('Launch ansible playbook') {
            steps {
                ansiblePlaybook installation: 'Ansible', inventory: 'inventory.yml', playbook: 'playbook.yml'
            }
        }
        stage('Test') {
            steps {
                httpRequest consoleLogResponseBody: true, responseHandle: 'NONE', timeout: 10, url: ' http://10.0.2.15:8081', validResponseCodes: '200', wrapAsMultipart: false
            }
        }

    }
    post {
        always {
            echo "finished deploying and testing"
        }
        failure {
            echo "failure"
        }
    }
}


