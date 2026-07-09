pipeline {
    agent any

    environment {
        MONGO_URI = credentials('mongo-uri')
        SECRET_KEY = credentials('secret-key')
        IMAGE_NAME = 'flask-app'
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

        stage('Test') {
            steps {
                sh '''
                . venv/bin/activate

                pytest -v
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker stop flask-app || true
                docker rm flask-app || true

                docker run -d \
                  --name flask-app \
                  -p 5000:5000 \
                  -e MONGO_URI="$MONGO_URI" \
                  -e SECRET_KEY="$SECRET_KEY" \
                  ${IMAGE_NAME}:latest
                '''
            }
        }
    }

    post {
    success {
        mail(
            to: 'akash.agarwal99@gmail.com',
            subject: "SUCCESS: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Hello,

The Jenkins pipeline completed successfully.

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Status: SUCCESS

Build URL:
${env.BUILD_URL}

Regards,
Jenkins
"""
        )
    }

    failure {
        mail(
            to: 'akash.agarwal99@gmail.com',
            subject: "FAILED: ${env.JOB_NAME} #${env.BUILD_NUMBER}",
            body: """
Hello,

The Jenkins pipeline has failed.

Job: ${env.JOB_NAME}
Build Number: ${env.BUILD_NUMBER}
Status: FAILED

Please check the console output:

${env.BUILD_URL}

Regards,
Jenkins
"""
        )
    }
}
}
