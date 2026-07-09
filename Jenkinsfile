pipeline {
    agent any

    environment {
        MONGO_URI = credentials('mongo-uri')
        SECRET_KEY = credentials('secret-key')
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate

                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Create Environment') {
            steps {
                sh '''
                cat > .env <<EOF
MONGO_URI=$MONGO_URI
SECRET_KEY=$SECRET_KEY
EOF
                '''
            }
        }

        stage('Code Formatting') {
            steps {
                sh '''
                . venv/bin/activate
                black --check .
                '''
            }
        }

        stage('Lint') {
            steps {
                sh '''
                . venv/bin/activate
                pylint app.py
                '''
            }
        }

        stage('Security Scan') {
            steps {
                sh '''
                . venv/bin/activate
                bandit -r .
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                . venv/bin/activate
                pytest -v
                '''
            }
        }
    }
}
