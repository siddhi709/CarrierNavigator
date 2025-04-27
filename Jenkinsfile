pipeline {
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/siddhi709/CarrierNavigator.git'
            }
        }
        stage('Run Tests') {
            steps {
                sh '$VENV_DIR/bin/pytest tests/'
            }
        }

        stage('Run Application (Optional)') {
            when {
                branch 'main'
            }
            steps {
                sh '$VENV_DIR/bin/python app.py'
            }
        }
    }

    post {
        always {
            echo 'Cleaning up...'
            sh 'rm -rf $VENV_DIR'
        }
    }
}

