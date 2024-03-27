pipeline {
    agent any
    stages {
        stage('show list') {
            steps {
                sh 'ls -la'
            }
        }
    }
}
withCredentials([usernamePassword(credentialsId: "${jenkinsCredentialsId}", usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
    echo "username is $USERNAME"            
}