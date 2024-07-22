pipeline {
  agent any

  environment {
    DOCKER_CREDENTIALS_ID = 'dockerhub-credentials'
  }

  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/zubeirahamed/simple-flask-app.git'
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'python3 -m venv .venv'
        sh 'venv/bin/pip install -r requirements.txt'
      }
    }

    stage('Lint and Test') {
      steps {
        sh 'venv/bin/flake8 --max-line-length=88'
        sh 'venv/bin/pytest'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t python/simple-flask-app .'
      }
    }

    stage('Push Docker Image') {
      steps {
        script {
          docker.withRegistry('https://index.docker.io/v1/', DOCKER_CREDENTIALS_ID) {
            sh 'docker push zubeirahamed/simple-flask-app'
          }
        }
      }
    }

    stage('Deploy to AWS EC2') {
      steps {
        sshagent(['ec2-ssh-key']) {
          sh '''
          ssh -o StrictHostKeyChecking=no ubuntu@100.25.162.30 '
            docker pull zubeirahamed/simple-flask-app
            docker stop $(docker ps -a -q) || true
            docker run -d -p 80:5000 python/simple-flask-app
          '
          '''
        }
      }
    }
