pipeline {
    agent any
    environment {
        VIRTUAL_ENV = 'venv'
    }
    stages {
        stage('Setup') {
            steps {
                script {
                    // Check if the virtual environment exists
                    if (!fileExists("${env.WORKSPACE}\\${VIRTUAL_ENV}")) {
                        bat "python -m venv ${VIRTUAL_ENV}"
                    }
                }
                // Activate the virtual environment and install requirements
                bat """
                    call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                    pip install -r requirements.txt
                """
            }
        }
        stage('Lint') {
            steps {
                // Run flake8 within the activated virtual environment
                bat """
                    call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                    flake8 app.py
                """
            }
        }
        stage('Test') {
            steps {
                // Run pytest within the activated virtual environment
                bat """
                    call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                    pytest
                """
            }
        }
        stage('Coverage') {
            steps {
                // Run coverage analysis within the activated virtual environment
                bat """
                    call ${VIRTUAL_ENV}\\Scripts\\activate.bat
                    coverage run -m pytest
                    coverage report
                """
            }
        }
        stage('Deploy') {
            steps {
                echo "Deploying application..."
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
