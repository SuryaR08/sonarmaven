pipeline {
    agent any
   /* tools{
        maven 'sonarmaven'   (we can also use this if maven set as install automatically in jenkins configurations)
    } */
    environment {
        MAVEN_PATH = 'C:\\Users\\surya\\Downloads\\apache-maven-3.9.9-bin\\apache-maven-3.9.9\\bin' 
        SONAR_TOKEN = credentials('sonarqube-token') 
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build & Test') {
            steps {
                bat '''
                set PATH=%MAVEN_PATH%;%PATH%
                mvn clean verify
                '''
            }
        }
        stage('SonarQube Analysis') {
            steps {
                bat '''
                set PATH=%MAVEN_PATH%;%PATH%
                mvn sonar:sonar ^
                  -Dsonar.projectKey=sonarmaven ^
                  -Dsonar.sources=src/main/java ^
                  -Dsonar.host.url=http://localhost:9000 ^
                  -Dsonar.token=%SONAR_TOKEN%
                '''
            }
        }
    }
    post {
        success {
            echo 'Build and SonarQube analysis succeeded!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
