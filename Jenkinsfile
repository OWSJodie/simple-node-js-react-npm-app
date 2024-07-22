pipeline {
    agent any
    tools {
        nodejs 'NodeJS 14' // This should match the name of your NodeJS installation
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}
