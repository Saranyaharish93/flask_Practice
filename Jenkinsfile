pipeline {
    agent any

       environment {
        MONGO_URI = 'mongodb+srv://saranyasmiles55_db_user:kG2GKIaveMNbW0fb@clustersaro.gdsocya.mongodb.net/studentsdb?retryWrites=true&w=majority&appName=ClusterSaro'
        SECRET_KEY = 'mysecretkey'
    }
    
    stages {
        stage('Build') {
            steps {
                echo 'Installing dependencies...'
                bat 'python -m venv venv'
                bat 'venv\\Scripts\\pip install -r requirements.txt'
            }
        }

        stage('Test') {
            steps {
                echo 'Running unit tests...'
                bat 'venv\\Scripts\\python -m pytest'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to staging...'
                bat 'start /B venv\\Scripts\\python app.py'
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