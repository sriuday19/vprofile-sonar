pipeline {
    agent any

    tools {
        jdk "OracleJDK17"
        maven "MAVEN3"
    }
    environment {
        scannerHome = tool 'sonar5'
        image_registry = "sriuday19/vprofile-kube"
        image_name= 'vprofile'
        docker_cred = 'docker-login'
        docker_url = 'https://hub.docker.com/repository/docker/sriuday19/vprofile-kube'
    }

    stages {

        stage('Build code') {
            steps {
                sh 'mvn clean install -DskipTests'
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

        stage('Build image') {
            steps {
                script {
                    dockerImage = docker.build("${image_registry}:${env.BUILD_NUMBER}", "./Dockerfiles/app/")
                }
                
            }
        }

        stage('pushing the image to docker hub') {
            steps {
                script {
                docker.withRegistry('', docker_cred) {
                     dockerImage.push("${image_name}-v${env.BUILD_NUMBER}-latest")
                }
               
                }
            }
        }
    }
}