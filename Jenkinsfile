pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    stages {

        stage('Checkout') {
            steps {
              checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'gitcred', url: 'https://github.com/Satya-satya989/netflix.git']])
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

        stage('Docker Build') {
            steps {
                sh 'docker build -t swiggy-app .'
            }
        }

        stage('Docker Run') {
            steps {
                sh 'docker stop swiggy-container || true'
                sh 'docker rm swiggy-container || true'
                sh 'docker run -d -p 3010:80 --name swiggy-container swiggy-app'
            }
        }
    }
}
