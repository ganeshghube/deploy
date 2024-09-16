pipeline {
    agent any

    stages {
         stage('Clean Repo') {
            steps {
                sh 'sudo rm -rf *'
            }
        }
        stage('Checkout Code') {
            steps {
                sshagent(['root']) {
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
