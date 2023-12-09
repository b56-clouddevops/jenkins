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
        // booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
        // choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
        // password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    }


    triggers { cron('*/1 * * * *') }

    stages {
        stage('Name of the stage - 1') {
            steps {
                sh 'echo how are you doing'
                sh "echo Name of the variable is ${ENV_URL}"
                sh "env"
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