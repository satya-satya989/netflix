pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        NODE_OPTIONS = "--openssl-legacy-provider"
        CI = "false"
    }

    stages {

        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Build React App') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Verify Dockerfile') {
            steps {
                sh 'ls -la Dockerfile || echo "Dockerfile NOT FOUND ❌"'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t swiggy-app .'
            }
        }

        stage('Docker Run') {
            steps {
                sh '''
                    docker stop swiggy-container || true
                    docker rm swiggy-container || true
                    docker run -d -p 3010:80 --name swiggy-container swiggy-app
                '''
            }
        }
    }
}
