pipeline {
    agent any
    tools {
        maven 'Maven 3.9'   // Make sure this matches Jenkins Global Tool Configuration
        jdk 'JDK17'
         // Make sure this matches Jenkins Global Tool Configuration
    }
    stages {
        stage('Clean Workspace') {
            steps {
                echo 'Cleaning workspace...'
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
                echo 'Checking out source code from GitHub...'
                checkout scm
            }
        }
        stage('Build') {
            steps {
                echo 'Building the Java application with Maven...'
                script {
                    if (isUnix()) {
                        sh 'mvn clean compile'
                    } else {
                        bat 'mvn clean compile'
                    }
                }
            }
        }
        stage('Unit Test') {
            steps {
                echo 'Running unit tests...'
                script {
                    if (isUnix()) {
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'
                    }
                }
            }
            post {
                always {
                    echo 'Archiving test results...'
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Package') {
            steps {
                echo 'Packaging the application...'
                script {
                    if (isUnix()) {
                        sh 'mvn package -DskipTests'
                    } else {
                        bat 'mvn package -DskipTests'
                    }
                }
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                }
            }
        }
    }
    post {
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}
