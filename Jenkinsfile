pipeline {
    agent any

    environment {
        // Defining the virtual environment directory
        VENV_DIR = 'venv'
        // Path to store the test results if needed
        TEST_RESULTS = 'test-results.xml'
    }

    stages {
        // Checkout Code from Git repository
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout code from the main branch
                    echo 'Checking out code from GitHub'
                    git branch: 'main', url: 'https://github.com/siddhi709/CarrierNavigator.git'
                }
            }
        }

        // Set up Python environment (install dependencies)
        stage('Set up Python') {
            steps {
                script {
                    echo 'Setting up Python environment'
                    // Ensure python3 and venv are available
                    sh 'python3 -m venv $VENV_DIR'  // Create virtual environment
                    sh '$VENV_DIR/bin/pip install --upgrade pip'  // Upgrade pip
                    sh '$VENV_DIR/bin/pip install -r requirements.txt'  // Install project dependencies
                    sh '$VENV_DIR/bin/pip install pytest'  // Install pytest for testing
                }
            }
        }

        // Run the tests using pytest
        stage('Run Tests') {
            steps {
                script {
                    echo 'Running tests using pytest'
                    // Run tests and generate XML results
                    sh '$VENV_DIR/bin/pytest --maxfail=1 --disable-warnings -q > $TEST_RESULTS'
                }
            }
        }

        // Run the application (only on the main branch)
        stage('Run Application') {
            when {
                branch 'main'
            }
            steps {
                script {
                    echo 'Running application'
                    // Run the application using the virtual environment python
                    sh '$VENV_DIR/bin/python app.py'
                }
            }
        }
    }

    // After the main pipeline runs
    post {
        always {
            echo 'Cleaning up...'
            // Clean up by removing the virtual environment
            sh 'rm -rf $VENV_DIR'
        }

        success {
            echo 'Build and tests succeeded!'
        }

        failure {
            echo 'Build or tests failed! Check the logs.'
            // Archive the test results if needed
            archiveArtifacts allowEmptyArchive: true, artifacts: "$TEST_RESULTS", onlyIfSuccessful: false
        }
    }
}
