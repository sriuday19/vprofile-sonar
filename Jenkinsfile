pipeline {
    agent any

    tools {
        jdk "OracleJDK17"
        maven "MAVEN3"
    }
    environment {
        scannerHome = tool 'sonar5'
    }

    stages {
        stage('Tests') {
            steps {
                sh 'mvn test'
            }
        }

        stage('checkstyle') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }

        stage('Sonarcloud') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        stage('Quality gate') {
            steps {
              timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
              }
        }
    }
}