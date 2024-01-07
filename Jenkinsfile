pipeline {
    agent any

    tools {
        jdk "OracleJDK8"
        maven "MAVEN3"
    }

    stages {

        stage('build code') {

            steps {
                sh 'mvn clean install'
            }
        }

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
    }
}