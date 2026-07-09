pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                echo 'Creating Python virtual environment'

                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                echo 'Running pytest'

                sh '''
                . venv/bin/activate
                pytest -v
                '''
            }
        }

    }
}
