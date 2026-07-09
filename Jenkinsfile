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
