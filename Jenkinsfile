pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'JDK_1.8'
    }
    environment {
        SONARQUBE_SCANNER_HOME = tool 'SonarQube-Scanner'
        SONAR_TOKEN = credentials('sonarqube-token')
    }
    stages {
        stage('Checkout') {
            steps {
                // Checkout code from GitHub
                checkout scm
            }
        }
        stage('Build') {
            steps {
                // Build project using Maven
                sh 'mvn clean compile'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv('SonarQube') {
                    bat '''
                    mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=sonar-maven2 \
                        -Dsonar.projectName='sonar-maven2' \
                        -Dsonar.host.url=http://localhost:9000 \
                        -Dsonar.token=%SONAR_TOKEN%
                    '''
                }
            }
        }
        
    }
  post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
        always {
            echo 'This runs regardless of the result.'
        }
    }
}
