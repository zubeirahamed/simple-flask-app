# Simple Flask Application

## Introduction

This is a simple Flask application deployed using a CI/CD pipeline with Jenkins.

## How to Run Locally

1. Clone the repository:
   ```bash
   git clone https://github.com/yourusername/simple-flask-app.git
   cd simple-flask-app
2. Set up a virtual environment:
   python3 -m venv .venv
   . .venv/bin/activate
3. Install dependencies:
   pip install -r requirements.txt
4. Run the application:
   python app.py

## How to Build and Run the Docker Container

1. Create a Dockerfile (added in repository)
2. Build the Docker image:
   docker build -t python/simple-flask-app .
3. Run the Docker container:
   docker run -p 80:5000 python/simple-flask-app
4. Access the application at:
   http://localhost:80

## CI/CD Pipeline with Jenkins
The CI/CD pipeline is configured using Jenkins. It includes the following steps:

1. Linting with flake8
2. Running unit tests with pytest
3. Building the Docker image
4. Pushing the Docker image to Docker Hub
5. Deploy to AWS EC2

## Configure CI/CD pipeline

1. Install Jenkins:
   You can install Jenkins on your on an AWS EC2 instance, or any other server.
   Follow the installation instructions for your operating system on the Jenkins official website.
2. Start Jenkins:
   After installation, start Jenkins
   for Linux:
   sudo systemctl start jenkins
3. Access Jenkins:
   Open your web browser and go to http://localhost:8080 (or the public IP address if installed on a remote server).

## Follow the setup instructions and install the suggested plugins.

  1. Create a New Pipeline Job:
     In the Jenkins dashboard, click on "New Item".
     Enter a name for your job (e.g., "Simple Flask App Pipeline").
     Select "Pipeline" and click "OK".
  2. Configure Jenkins Pipeline
     Configure Git Repository:
     In the job configuration, under "Pipeline", select "Pipeline script from SCM".
     Choose "Git" and enter repository URL https://github.com/zubeirahamed/simple-flask-app.git
  3. Add Docker Hub Credentials:
     In the Jenkins dashboard, go to "Manage Jenkins" > "Manage Credentials".
     Click on "(global)" under "Stores scoped to Jenkins".
     Click "Add Credentials" and choose "Username with password".
     Enter your Docker Hub username and password, and give it an ID "dockerhub-credentials".
  4. Set Up SSH Key on Jenkins:
     Add your EC2 instance SSH key to Jenkins. You can do this in the Jenkins dashboard:
     Go to "Manage Jenkins" > "Manage Credentials".
     Click on "(global)" under "Stores scoped to Jenkins".
     Click "Add Credentials" and choose "SSH Username with private key".
     Enter the ID "ec2-ssh-key", your SSH username (e.g., ec2-user), and the private key.
  5. Install Docker and SSH Agent Plugin in Jenkins:
     Go to "Manage Jenkins" > "Manage Plugins".
     Search for "Docker" and "SSH" Plugins and install it without restarting Jenkins.  
  6. Run the Pipeline:
     Go to your Jenkins job and click "Build Now".
     Jenkins will execute the stages defined in the Jenkinsfile (added in repository).

     
