# Flask Application CI/CD Pipeline using Jenkins

## Project Overview

This project demonstrates Continuous Integration and Continuous Deployment (CI/CD) for a Flask application using Jenkins. The pipeline automates dependency installation, testing, deployment, and email notifications.

---

## Technologies Used

- Python 3.12
- Flask
- Jenkins
- GitHub
- Pytest
- Gmail SMTP

---

## Prerequisites

Install the following software:

- Python
- Git
- Jenkins
- Java JDK
- Required Jenkins Plugins:
  - Git Plugin
  - Pipeline Plugin
  - Email Extension Plugin

---

## Step 1: Verify Python Installation

Open Command Prompt and verify:

cmd
python --version
pip --version


Expected output:

cmd
Python 3.12.x
pip 26.x


---

## Step 2: Install Project Dependencies

Navigate to project folder:

cmd
cd C:\Users\Harish Saranya\Documents\flask_Practice


Install dependencies:

cmd
pip install -r requirements.txt
pip install pytest


Verify pytest installation:

cmd
pytest --version


---

## Step 3: Configure Jenkins Email Notification

Open Jenkins:

text
http://localhost:8080


Navigate to:

text
Manage Jenkins → System


### Extended E-mail Notification Configuration

Configure:

text
SMTP Server : smtp.gmail.com
SMTP Port   : 587
Credentials : Gmail Account Credential
Use SSL     : No
Use TLS     : Yes


### E-mail Notification Configuration

Configure:

text
SMTP Server            : smtp.gmail.com
Use SMTP Authentication: Enabled
Username               : Gmail Address
Password               : Gmail App Password
Use TLS                : Enabled
Use SSL                : Disabled
SMTP Port              : 587
Charset                : UTF-8


Save the configuration.

---

## Step 4: Create Gmail App Password

Normal Gmail password cannot be used.

Steps:

1. Open Google Account.
2. Navigate to Security.
3. Enable 2-Step Verification.
4. Open App Passwords.
5. Select:
   - App: Mail
   - Device: Other (Jenkins)
6. Generate App Password.
7. Copy the generated 16-character password.
8. Use this password in Jenkins email configuration.

---

## Step 5: Create Jenkins Pipeline Job

Open Jenkins Dashboard.

Click:

text
New Item


Enter:

text
flask-jenkins-pipeline


Select:

text
Pipeline


Click:

text
OK


Configure:

text
Pipeline Definition:
Pipeline script from SCM

SCM:
Git

Repository URL:
https://github.com/<username>/flask_Practice.git

Branch:
main

Script Path:
Jenkinsfile


Save the job.

---

## Step 6: Create Jenkinsfile

Create a file named:

text
Jenkinsfile


Add the following content:

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

---

## Step 7: Push Code to GitHub

Open terminal:

cmd
git add .
git commit -m "Add Jenkins CI/CD pipeline"
git push origin main


---

## Step 8: Execute Jenkins Pipeline

Open Jenkins job:

text
flask-jenkins-pipeline


Click:

text
Build Now


Pipeline stages executed:

text
Build
   ↓
Test
   ↓
Deploy


## Common Issues and Solutions

### Issue 1



text
Connection refused: localhost:25


Reason:

text
SMTP configuration missing.


Solution:

text
Configure smtp.gmail.com with port 587.
Enable TLS.


---
### Issue 2

Error:

text
530-5.7.0 Must issue a STARTTLS command first


Reason:

text
is disabled.


Solution:

text
Enable TLS.
Disable SSL.
Use SMTP port 587.


## Project Workflow

text
Developer Pushes Code
            ↓
        GitHub
            ↓
        Jenkins
            ↓
      Build Stage
            ↓
      Test Stage
            ↓
     Deploy Stage
            ↓
 Email Notification


## Expected Outcome

After successful execution:

- Source code is pulled from GitHub.
- Python dependencies are installed.
- Unit tests are executed.
- Flask application is deployed.
- Email notification is sent.
- Jenkins build status is updated.
