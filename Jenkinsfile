pipeline {
    agent { docker { image 'node:20.11.1-alpine3.19' } }
    stages {
        stage('show list') {
            steps {
                sh 'ls -la'
            }
        }
    }
}