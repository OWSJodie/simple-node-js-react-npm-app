pipeline {
    agent any

    tools {nodejs "NodeJs"}
    
    stages {
        stage ('Checkout') {
            steps {
                git branch:'master', url: 'https://github.com/OWSJodie/simple-node-js-react-npm-app.git'
            }
        }
        
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }

        stage('OWASP Dependency-Check Vulnerabilities') {
          steps {
            dependencyCheck additionalArguments: ''' 
                        -o './'
                        -s './'
                        -f 'ALL' 
                        --prettyPrint''', odcInstallation: 'OWASP Dependency-Check Vulnerabilities'
            
            dependencyCheckPublisher pattern: 'dependency-check-report.xml'
          }
        }

        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                def scannerHome = tool 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                    sh "/var/jenkins_home/tools/hudson.plugins.sonar.SonarRunnerInstallation/SonarQube/bin/sonar-scanner -Dsonar.projectKey=OWASP -Dsonar.sources=. -Dsonar.host.url=http://192.168.0.228:9000 -Dsonar.token=sqp_7ddbfdb6dc058ef73e9ec4c0bedd7dfbd063bf88"
                    }
                }
            }
        }
    }
     post {
        always {
            recordIssues enabledForFailure: true, tool: sonarQube()
        }
    }
}

