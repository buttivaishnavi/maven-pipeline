pipeline {
    agent any

    tools {
        jdk 'JDK21'     // Must match the name in Jenkins → Manage Jenkins → Tools
        maven 'Maven3'  // Must match your configured Maven name
    }

    stages {
        stage('Checkout') {
            steps {
                // Clones your GitHub repository
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }

        stage('Build & Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn package'
                        sh 'mvn test'
                    } else {
                        bat 'mvn package'
                        bat 'mvn test'
                    }
                }
            }
        }

        stage('Archive Artifact') {
            steps {
                // Collect test results and archive jar file
                junit 'target/surefire-reports/*.xml'
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo '✅ Build succeeded!'
        }
        failure {
            echo '❌ Build failed. Please check logs.'
        }
    }
}
