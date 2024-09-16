pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                sshagent(['git']) {
                    // Replace with your Git repository's SSH URL
                    git branch: 'main', url: 'git@github.com:ganeshghube/deploy.git'
                }
            }
        }

        stage('Build') {
            steps {
                sh 'pwd'
            }
        }

        stage('Test') {
            steps {
                sh 'pwd'
            }
        }
    }
    
    post {
        always {
            echo 'Pipeline completed.'
        }
    }
}
