pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Checking out latest code from GitHub...'

                checkout scmGit(
                    branches: [[name: 'main']],
                    userRemoteConfigs: [[
                        credentialsId: 'github-ssh',
                        url: 'git@github.com:Tanya-verma06/Flask-App-LabTask.git'
                    ]]
                )
            }
        }

        stage('Setup Virtual Environment') {
            steps {
                echo 'Creating Python virtual environment...'

                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    python --version
                    pip --version
                '''
            }
        }

        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies from requirements.txt...'

                sh '''
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Build / Test Flask Application') {
            steps {
                echo 'Testing Flask application...'

                sh '''
                    . venv/bin/activate
                    python -m py_compile app.py
                '''
            }
        }

        stage('Run Flask Application') {
            steps {
                echo 'Starting Flask application...'

                sh '''
                    . venv/bin/activate
                    timeout 10 python app.py || true
                '''
            }
        }
    }

    post {
        success {
            echo 'Flask CI Pipeline completed successfully!'
        }

        failure {
            echo 'Flask CI Pipeline failed!'
        }
    }
}
