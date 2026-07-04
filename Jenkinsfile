pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                sh 'python3 -m venv venv'
                sh '. venv/bin/activate && pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                sh '. venv/bin/activate && pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to staging...'
                sh 'pkill -f app.py || true'
                sh '. venv/bin/activate && nohup python app.py > flask.log 2>&1 &'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
            mail to: 'saranya.smiles@gmail.com',
                 subject: 'Jenkins Pipeline Success',
                 body: 'Build, test and deployment completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
            mail to: 'saranya.smiles@gmail.com',
                 subject: 'Jenkins Pipeline Failed',
                 body: 'Jenkins pipeline failed. Please check console output.'
        }
    }
}