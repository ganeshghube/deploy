pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                deleteDir() // Clean workspace to avoid corrupted files
            }
        }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the code from GitHub using credentials
                git clone 'https://github.com/ganeshghube/deploy.git'
            }
        }

        stage('Build') {
            steps {
                // Add your build steps here
                // For example, if it's a Maven project:
                sh 'pwd'
            }
        }

        stage('Test') {
            steps {
                // Add test steps here
                // For example, if it's a Maven project with tests:
                sh 'pwd'
            }
        }

        stage('Deploy') {
            steps {
                // Add deployment steps here (optional)
                echo 'Deploying application...'
            }
        }
    }

    post {
        always {
            // Cleanup steps or notifications can go here
            echo 'Pipeline completed.'
        }
    }
}
