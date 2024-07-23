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
        sh 'chmod -R 755 .venv'
        sh 'chown -R jenkins:jenkins .venv'
        sh 'chmod +x .venv/bin/pip'
        sh '. .venv/bin/activate && .venv/bin/python -m pip install --break-system-packages -r requirements.txt'
      }
    }

    stage('Lint and Test') {
      steps {
        sh '. .venv/bin/activate && .venv/bin/python -m flake8 --exclude=.venv --max-line-length=88 app.py'
        sh '. .venv/bin/activate && .venv/bin/python -m pytest'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t zubeirahamed/simple-flask-app .'
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
            docker run -d -p 80:5000 zubeirahamed/simple-flask-app
          '
          '''
        }
      }
    }
  }
}
