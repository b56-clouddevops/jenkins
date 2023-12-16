node {
    stage('Test') {
        print 'Hello World'
    }
    if(env.TAG_NAME == "") {
        stage('Runs On Tag Name') {
            print 'Runs on Tag'
        }
    else {
            stage('Runs On Branch') {
            p   rint 'Runs on Branch'
            } 
        }
    }
}