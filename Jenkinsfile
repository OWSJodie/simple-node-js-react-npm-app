pipeline {
    agent any

    tools {
        nodejs "NodeJs"  // Ensure this is correctly installed in Jenkins
    }

    environment {
        SONAR_TOKEN = credentials('sonar-token')  // Use Jenkins credentials plugin to store the token securely
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/OWSJodie/simple-node-js-react-npm-app.git'
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm install'  // Ensure package.json exists in the repo
            }
        }

        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    def scannerHome = tool name: 'SonarQube', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                    withSonarQubeEnv('SonarQube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://192.168.0.228:9000 -Dsonar.token=${env.SONAR_TOKEN}"
                    }
                }
            }
        }
    }

    post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube(pattern: '**/sonar-report.json')
        }
    }
}
