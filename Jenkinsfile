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
                sh './gradlew clean check'
            }
        }

        stage('Test Results') {
            steps {
                junit allowEmptyResults: true, testResults: '**/build/test-results/test/*.xml'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Build succeeded!'
        }
        failure {
            echo 'Build failed. Check console output for details.'
        }
    }
}

}
