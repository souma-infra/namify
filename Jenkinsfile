pipeline {
    agent any

    options {
        timestamps()
        timeout(time: 10, unit: 'MINUTES')
    }

    environment {
        APP_NAME = 'namify'
        BUILD_TAG_CUSTOM = "${APP_NAME}-${BUILD_NUMBER}"
    }

    stages {
        stage('Checkout') {
            steps {
                echo "Starting build #${BUILD_NUMBER} for job ${JOB_NAME}"
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                echo "Installing dependencies for ${APP_NAME}..."
                sh 'pip3 install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo "Running tests, build tag: ${BUILD_TAG_CUSTOM}"
                sh 'pytest test_app.py --junitxml=results.xml'
            }
        }
    }

    post {
        always {
            echo "Pipeline finished for build #${BUILD_NUMBER}"
            junit 'results.xml'
        }
        success {
            echo "✅ Build ${BUILD_TAG_CUSTOM} succeeded."
        }
        failure {
            echo "❌ Build ${BUILD_TAG_CUSTOM} failed. Check the Test stage output above."
        }
    }
}
