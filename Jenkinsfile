pipeline {
    agent any

    tools {
        maven 'Maven 3.8.7' // Make sure this is installed in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/openai/jenkins-maven-demo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
                junit 'target/surefire-reports/*.xml'
            }
        }
    }
}
