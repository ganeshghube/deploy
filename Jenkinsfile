pipeline {
    agent any
    
    environment {
        GITHUB_TOKEN = credentials('jenkins-user')
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: "https://${GITHUB_TOKEN}@github.com/ganeshghube/deploy.git
            }
        }
        
        stage('Build') {
            steps {
                // Add your build steps here
                sh 'echo "Building the project"'
            }
        }
        
        stage('Test') {
            steps {
                // Add your test steps here
                sh 'echo "Running tests"'
            }
        }
        
        stage('Deploy') {
            steps {
                // Add your deployment steps here
                sh 'echo "Deploying the application"'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline execution failed.'
        }
    }
}