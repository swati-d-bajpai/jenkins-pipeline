pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/swati-d-bajpai/jenkins-pipeline.git'
            }
        }
        stage('Install') {
            steps {
                sh 'cd backend'
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'cd backend'
                sh 'npm run build'
            }
        }
    }
}