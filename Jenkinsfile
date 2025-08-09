pipeline {
    agent {
        docker {
            image 'gradle:8.4-jdk17'
            args '-v $HOME/.gradle:/home/gradle/.gradle'
        }
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        timestamps()
    }

    environment {
        GRADLE_OPTS = "-Dorg.gradle.daemon=false"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Publish Results') {
            steps {
                junit allowEmptyResults: true, testResults: '**/build/test-results/test/*.xml'
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed. Please check the console output.'
        }
        always {
            echo '🧹 Cleaning up workspace...'
        }
    }
}
