
# AWS Python CI/CD Project

This repository contains a simple Python Flask application with a fully automated **CI/CD pipeline** using **AWS CodePipeline**, **CodeBuild**, and **Docker**. The application is designed to demonstrate modern DevOps practices for building, testing, and deploying containerized applications.

---

## Project Overview

- **Application:** Python Flask "Hello World" web app
- **Containerization:** Docker
- **CI/CD Pipeline:** GitHub → AWS CodePipeline → AWS CodeBuild → Docker Hub
- **Goal:** Automate the build and push of Docker images whenever the code changes in GitHub

---

## Features

- Python Flask web application
- Dockerized for easy deployment
- Automated build pipeline triggered by GitHub commits
- Secure management of Docker credentials using **AWS Parameter Store**
- CI/CD workflow without exposing secrets in the repository

---

## Project Structure

aws-devops-learn/
│
├── cicd-pipeline-using-aws-service/
│ └── my-python-app/
│ ├── app.py # Flask application
│ ├── requirements.txt
│ └── Dockerfile
├── buildspec.yml # CodeBuild instructions
└── README.md

yaml
Copy code

---

## How It Works

1. **Source Control:** The pipeline monitors this GitHub repository for any code changes.
2. **CI/CD Trigger:** When `app.py` (or other tracked files) are updated, CodePipeline automatically triggers.
3. **CodeBuild Stage:** 
   - Fetches code from GitHub
   - Installs Python dependencies
   - Builds the Docker image
   - Authenticates with Docker Hub
   - Pushes the Docker image to your Docker Hub repository
4. **Deployment Stage:** *(To be added later using AWS CodeDeploy / other methods)*

---

## Challenges Faced

During implementation, the following challenges were encountered:

1. **Docker Hub Authentication**
   - Initially, the pipeline failed to push images because Docker was not logged in.
   - Solved by securely storing Docker credentials in **AWS Parameter Store** and using `docker login --password-stdin` in `pre_build`.

2. **Docker Repository Naming**
   - Attempting to push to an incorrectly structured repository path caused access denied errors.
   - Fixed by ensuring the repository existed in Docker Hub and using the correct `username/repo` naming convention.

3. **AWS Permissions**
   - CodeBuild needed permission to read parameters from SSM Parameter Store.
   - Resolved by attaching the proper IAM policy to the CodeBuild service role.

4. **Free Tier Limitations**
   - CodeDeploy could not be used immediately due to account setup restrictions.
   - Temporarily removed deployment stages and cleaned up resources to avoid accidental charges.

---

## How to Run Locally

```bash
# Clone the repo
git clone https://github.com/<your-username>/<repo-name>.git

# Navigate to app directory
cd cicd-pipeline-using-aws-service/my-python-app

# Create virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install -r requirements.txt

# Run the Flask app
python app.py

# Visit http://127.0.0.1:5000 in your browser
