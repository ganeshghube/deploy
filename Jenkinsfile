pipeline {
    agent any
    
    parameters {
        string(name: 'SERVER_NAME', defaultValue: 'new_server', description: 'Name of the server to add to Ansible inventory')
        string(name: 'SERVER_IP', defaultValue: '192.168.1.100', description: 'IP address of the server')
        string(name: 'ANSIBLE_USER', defaultValue: 'admin', description: 'Ansible user for the server')
    }
    
    environment {
        GITHUB_TOKEN = credentials('jenkins-user')
        ANSIBLE_INVENTORY = '/etc/ansible/hosts'
    }
    
    stages {
        stage('Clean') {
            steps {
                cleanWs()
            }
        }
        
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: "https://${GITHUB_TOKEN}@github.com/ganeshghube/deploy.git"
            }
        }
        
        stage('Update Ansible Inventory') {
            steps {
                script {
                    def newLine = "${params.SERVER_NAME} ansible_host=${params.SERVER_IP} ansible_user=${params.ANSIBLE_USER}"
                    
                    sh """
                    if [ -w ${ANSIBLE_INVENTORY} ]; then
                        sed -i '\$d' ${ANSIBLE_INVENTORY} && echo "${newLine}" >> ${ANSIBLE_INVENTORY}
                    else
                        echo "Need sudo rights to modify ${ANSIBLE_INVENTORY}"
                        sudo sed -i '\$d' ${ANSIBLE_INVENTORY} && echo "${newLine}" | sudo tee -a ${ANSIBLE_INVENTORY}
                    fi
                    """
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'echo "Building the project"'
            }
        }
        
        stage('Test') {
            steps {
                sh 'echo "Running tests"'
            }
        }
        
        stage('Deploy') {
            steps {
                sh 'echo "Deploying the application"'
            }
        }
    }
    
    post {
        success {
            echo 'Pipeline executed successfully!'
        }
    }
}