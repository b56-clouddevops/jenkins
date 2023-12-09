//  Reference : jenkins.io/doc/book/pipeline/syntax/ 

pipeline {
    agent any 
    environment {
        ENV_URL = "pipeline.google.com"                         // Pipeline variables ( Global )
        SSH_CRED = credentials('SSH_CRED')
    }
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
    }
    options { 
        buildDiscarder(logRotator(numToKeepStr: '10')) 
        timeout(time: 59, unit: 'MINUTES')
        } 

    tools {
        maven 'maven-390' 
    }

    triggers { pollSCM('*/1 * * * *') }
    
    stages {
        stage('Name of the stage - 1') {
            steps {
                sh "mvn --version"
                sh "echo Name of the variable is ${ENV_URL}"
                sh "env"
                sh "sleep 150"
            }
        }
        stage('Name of the stage - 2') {
            environment {
                ENV_URL = "stage.google.com"                    // task variable ( task varaibles will have higher priority than pipeline variable )
            }
            steps {
                sh "echo Name of the variable is ${ENV_URL}"
                sh "echo step1"
                sh "echo step2"
                sh "echo step3"
            }
        }
        stage('Name of the stage - 3') {
            steps {
                sh "echo step1"
                sh "echo step2"
                sh "echo step3"
            }
        }
    }
}