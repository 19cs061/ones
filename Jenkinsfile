pipeline {
    agent any

    environment {
        PYTHON_VERSION = '3.8'
        VENV = "${WORKSPACE}/venv"
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    sh "python${PYTHON_VERSION} -m venv ${VENV}"
                    sh "${VENV}/bin/pip install -r requirements.txt"
                }
            }
        }

        stage('Test') {
            steps {
                script {
                    sh "${VENV}/bin/pytest"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    sh "${VENV}/bin/uvicorn zip:app --host 0.0.0.0 --port 8000 &"
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
