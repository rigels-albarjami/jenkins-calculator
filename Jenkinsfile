pipeline {
    agent any

    stages {
        stage('Get the code') {
            steps {
                checkout scm
            }
        }
        stage('Install') {
            steps {
                sh 'python3 -m venv venv'
                sh 'venv/bin/pip install -r requirements.txt'
            }
        }
        stage('Test') {
            steps {
                sh 'venv/bin/pytest --junitxml=results.xml'
            }
        }
        stage('Run the app') {
            steps {
                sh 'venv/bin/python main.py'
            }
        }
    }

    post {
        always {
            junit 'results.xml'
        }
        success {
            echo 'Everything OK: all tests passed and the app runs!'
        }
        failure {
            echo 'Something broke: open the red stage and read the log.'
        }
    }
}
