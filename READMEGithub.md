  # Flask CI/CD Pipeline using GitHub Actions

## Project Overview

This project demonstrates the implementation of a Continuous Integration and Continuous Deployment (CI/CD) pipeline for a Flask application using GitHub Actions.

The pipeline automates:

- Installing project dependencies
- Running automated tests using Pytest
- Building the application
- Deploying to a staging environment
- Deploying to a production environment

---

## Repository

GitHub Repository:

https://github.com/Saranyaharish93/flask_Practice

---

## Technologies Used

- Python 3.10
- Flask
- MongoDB Atlas
- Pytest
- GitHub Actions
- GitHub Secrets

---

## Branch Strategy

The repository contains the following branches:

- main
- staging

Deployment strategy:

| Branch/Tag | Deployment Environment |
|------------|------------------------|
| staging | Staging |
| v*.*.* tag | Production |

---

## Project Structure

text
flask_Practice/
│
├── .github/
│   └── workflows/
│       └── flask-ci-cd.yml
│
├── templates/
├── static/
├── app.py
├── test_app.py
├── requirements.txt
├── README.md
└── .env


---

## GitHub Actions Workflow

The workflow file is located at:

text
.github/workflows/flask-ci-cd.yml


The workflow performs the following tasks:

### 1. Install Dependencies

yaml
python -m pip install --upgrade pip
pip install -r requirements.txt
pip install pytest


---

### 2. Run Automated Tests

The application tests are executed using Pytest.

yaml
pytest -v


---

### 3. Build Application

After successful tests, the build stage executes.

yaml
echo "Build completed successfully"


---

### 4. Deploy to Staging

Deployment to the staging environment occurs when code is pushed to:

text
staging


Example:

bash
git push origin staging


---

### 5. Deploy to Production

Deployment to the production environment occurs when a release tag is pushed.

Example:

bash
git tag v1.0.0
git push origin v1.0.0


---

## GitHub Secrets Configuration

Sensitive information is stored securely using GitHub Secrets.

Navigate to:

text
Repository
   ↓
Settings
   ↓
Secrets and variables
   ↓
Actions


Create the following secrets:

| Secret Name | Description |
|-------------|-------------|
| MONGO_URI | MongoDB Atlas Connection String |
| SECRET_KEY | Flask Secret Key |
| STAGING_DEPLOY_KEY | Staging Deployment Key |
| PRODUCTION_DEPLOY_KEY | Production Deployment Key |

Example:

text
MONGO_URI=mongodb+srv://username:password@cluster.mongodb.net/dbname
SECRET_KEY=mysecretkey
STAGING_DEPLOY_KEY=dummy-staging-key-123
PRODUCTION_DEPLOY_KEY=dummy-production-key-123


---

## Workflow Trigger Conditions

### Build and Test

Triggered on:

- Push to main branch
- Push to staging branch
- Pull request to main
- Pull request to staging

---

### Staging Deployment

Triggered only when:

yaml
github.ref == 'refs/heads/staging'


---


## CI/CD Workflow Execution Results

### Staging Deployment

Command:

bash
git push origin staging


Result:

text
Build-Test              SUCCESS
Deploy-Staging          SUCCESS
Deploy-Production       SKIPPED


---

### Production Deployment

Command:

bash
git tag v1.0.0
git push origin v1.0.0


Result:

text
Build-Test              SUCCESS
Deploy-Staging          SKIPPED
Deploy-Production       SUCCESS


---

## Conclusion

This project successfully demonstrates the implementation of a complete CI/CD pipeline for a Flask application using GitHub Actions. The workflow automatically performs testing, build validation, staging deployment, and production deployment while securely managing sensitive information using GitHub Secrets.
