pipeline {
    agent any
    tools {
        maven 'Maven'
        jdk 'JDK_1.8'
    }
    environment {
        
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
                bat 'mvn clean package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Run SonarQube analysis
                withSonarQubeEnv('SonarQube-Scanner') {
                    bat '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=sonar-maven \
                      -Dsonar.projectName='sonar-maven' \
                      -Dsonar.sources=src/main/java \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.login=sqp_ba8f59443b785a285d1a866a4d028541729fa5a5
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
