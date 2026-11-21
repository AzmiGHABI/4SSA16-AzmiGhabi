pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
                echo "Commit: ${env.GIT_COMMIT}"
            }
        }

        stage('Build Maven') {
            steps {
                dir('backend') {
                    sh 'chmod +x mvnw'
                    sh './mvnw -B -ntp clean package -DskipTests'
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'backend/target/*.jar', allowEmptyArchive: true
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    dir('backend') {
                        sh "./mvnw -B -ntp org.sonarsource.scanner.maven:sonar-maven-plugin:5.9.0.7382:sonar -Dsonar.host.url=http://localhost:9000 -Dsonar.token=${SONAR_TOKEN} -Dsonar.projectKey=azmighabi-backend -Dsonar.projectName=4SSA16-AzmiGHABI-backend"
                    }
                }
            }
        }
    }
}
