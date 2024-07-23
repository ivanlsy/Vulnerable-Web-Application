pipeline {
    agent any
    
    environment {
        // Define the SonarQube server URL and token
        SONARQUBE_SERVER = 'http://172.30.137.160:9000'
        SONARQUBE_TOKEN = 'sqp_d51157091234eebed90d6d058826cdd1b5e685dc'
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/ivanlsy/Vulnerable-Web-Application.git'
            }
        }
        
        stage('Code Quality Check via SonarQube') {
            steps {
                script {
                    // Ensure the tool name matches what is configured in Jenkins
                    def scannerHome = tool name: 'SonarQube';
                    withSonarQubeEnv('SonarQube') {
                        sh """
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey=OWASP \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=${env.SONARQUBE_SERVER} \
                        -Dsonar.login=${env.SONARQUBE_TOKEN} \
                        -Dsonar.verbose=true -X
                        """
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
