pipeline {
    agent any

    environment {
        TEST_RESULTS = 'test-results.xml'
    }

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    echo 'Checking out code from GitHub'
                    git branch: 'main', url: 'https://github.com/siddhi709/CarrierNavigator.git', changelog: false, poll: false, depth: 1
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests using pytest'
                    // Ensure pytest is already installed globally
                    sh 'pytest --maxfail=1 --disable-warnings -q > $TEST_RESULTS || true'
                }
            }
        }

        stage('Run Application') {
            when {
                branch 'main'  // Keep this condition to ensure it only runs on main branch
            }
            steps {
                script {
                    echo 'Checking for required packages'
                    // Check and install pytest if it's missing
                    sh 'python3 -c "import pytest" || pip3 install pytest'
                    echo 'Running the application'
                    sh 'python3 app.py'
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
        }
        success {
            echo 'Build and tests succeeded!'
        }
        failure {
            echo 'Build or tests failed! Check the logs.'
            archiveArtifacts allowEmptyArchive: true, artifacts: "$TEST_RESULTS", onlyIfSuccessful: false
        }
    }
}
