pipeline {
    agent any

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
MONGO_URI=mongodb+srv://admin:evzQj7s31johbKKx@akashcluster.yaoewss.mongodb.net/studentDB?retryWrites=true&w=majority&appName=AkashCluster
SECRET_KEY=jenkins-secret-key
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
