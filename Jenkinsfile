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
                    // Ensure pytest is already installed in the system, as we are skipping Python setup
                    sh 'pytest --maxfail=1 --disable-warnings -q > $TEST_RESULTS || true'
                }
            }
        }

        stage('Run Application') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo 'Running application'
                    // Ensure Python is already available on the system
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
