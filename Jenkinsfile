pipeline {
    agent any
    
    parameters {
        string(name: 'SERVER_NAME', defaultValue: 'new_server', description: 'Name of the server to add to Ansible inventory')
        string(name: 'SERVER_IP', defaultValue: '192.168.1.100', description: 'IP address of the server')
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
                    def newLine = "${params.SERVER_NAME} ansible_host=${params.SERVER_IP}"
                    
                    def exitCode = sh(script: """
                        if [ -w ${ANSIBLE_INVENTORY} ]; then
                            if grep -q "${params.SERVER_NAME}" ${ANSIBLE_INVENTORY}; then
                                echo "Error: Entry for ${params.SERVER_NAME} already exists in ${ANSIBLE_INVENTORY}"
                                exit 1
                            else
                                echo "${newLine}" >> ${ANSIBLE_INVENTORY}
                                echo "Successfully added ${params.SERVER_NAME} to ${ANSIBLE_INVENTORY}"
                            fi
                        else
                            if sudo grep -q "${params.SERVER_NAME}" ${ANSIBLE_INVENTORY}; then
                                echo "Error: Entry for ${params.SERVER_NAME} already exists in ${ANSIBLE_INVENTORY}"
                                exit 1
                            else
                                echo "${newLine}" | sudo tee -a ${ANSIBLE_INVENTORY}
                                echo "Successfully added ${params.SERVER_NAME} to ${ANSIBLE_INVENTORY}"
                            fi
                        fi
                    """, returnStatus: true)

                    if (exitCode != 0) {
                        error("Failed to update Ansible inventory")
                    }
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
        failure {
            echo 'Pipeline execution failed.'
        }
    }
}