pipeline {
    agent any

    parameters {
        string(name: 'SERVER_NAME', defaultValue: 'apimanager', description: 'Name of the server to add to Ansible inventory and /etc/hosts')
        string(name: 'SERVER_IP', defaultValue: '192.168.1.250', description: 'IP address of the server')
    }

    environment {
        GITHUB_TOKEN = credentials('jenkins-user')   // GitHub access token for repo checkout
        ANSIBLE_INVENTORY = '/etc/ansible/hosts'     // Path to Ansible inventory
        HOSTS_FILE = '/etc/hosts'                    // Path to /etc/hosts file
    }

    stages {
        stage('Clean') {
            steps {
                cleanWs() // Clean the workspace
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
                    def newInventoryEntry = "${params.SERVER_NAME} ansible_host=${params.SERVER_IP}"
                    
                    def inventoryExitCode = sh(script: """
                        if [ -w ${ANISBLE_INVENTORY} ]; then
                            if grep -q "^${params.SERVER_NAME}" ${ANISBLE_INVENTORY}; then
                                echo "Error: Entry for ${params.SERVER_NAME} already exists in ${ANISBLE_INVENTORY}"
                                exit 1
                            else
                                echo "${newInventoryEntry}" >> ${ANISBLE_INVENTORY}
                                echo "Successfully added ${params.SERVER_NAME} to ${ANISBLE_INVENTORY}"
                            fi
                        else
                            if sudo grep -q "^${params.SERVER_NAME}" ${ANISBLE_INVENTORY}; then
                                echo "Error: Entry for ${params.SERVER_NAME} already exists in ${ANISBLE_INVENTORY}"
                                exit 1
                            else
                                echo "${newInventoryEntry}" | sudo tee -a ${ANISBLE_INVENTORY}
                                echo "Successfully added ${params.SERVER_NAME} to ${ANISBLE_INVENTORY}"
                            fi
                        fi
                    """, returnStatus: true)

                    if (inventoryExitCode != 0) {
                        error("Failed to update Ansible inventory")
                    }
                }
            }
        }

        stage('Update /etc/hosts') {
            steps {
                script {
                    def newHostsEntry = "${params.SERVER_IP} ${params.SERVER_NAME}"
                    
                    def hostsExitCode = sh(script: """
                        if [ -w ${HOSTS_FILE} ]; then
                            if grep -q "^${params.SERVER_IP}" ${HOSTS_FILE}; then
                                echo "Error: Entry for ${params.SERVER_IP} already exists in ${HOSTS_FILE}"
                                exit 1
                            else
                                echo "${newHostsEntry}" >> ${HOSTS_FILE}
                                echo "Successfully added ${params.SERVER_IP} to ${HOSTS_FILE}"
                            fi
                        else
                            if sudo grep -q "^${params.SERVER_IP}" ${HOSTS_FILE}; then
                                echo "Error: Entry for ${params.SERVER_IP} already exists in ${HOSTS_FILE}"
                                exit 1
                            else
                                echo "${newHostsEntry}" | sudo tee -a ${HOSTS_FILE}
                                echo "Successfully added ${params.SERVER_IP} to ${HOSTS_FILE}"
                            fi
                        fi
                    """, returnStatus: true)

                    if (hostsExitCode != 0) {
                        error("Failed to update /etc/hosts")
                    }
                }
            }
        }

        stage('SSH Key Distribution') {
            steps {
                script {
                    withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key-id', keyFileVariable: 'SSH_PRIVATE_KEY'), 
                                     usernamePassword(credentialsId: 'linuxpass', usernameVariable: 'LINUX_USER', passwordVariable: 'ROOT_PASS')]) {
                        // Using ssh-keyscan to ensure the host is known
                        sh """
                            ssh-keyscan -H ${params.SERVER_IP} >> ~/.ssh/known_hosts
                        """
                        
                        // Perform ssh-copy-id using the provided SSH key and user
                        def sshKeyExitCode = sh(script: """
                            sshpass -p ${ROOT_PASS} ssh-copy-id -i ${SSH_PRIVATE_KEY} ${LINUX_USER}@${params.SERVER_IP}
                        """, returnStatus: true)

                        if (sshKeyExitCode != 0) {
                            error("Failed to distribute SSH key to ${params.SERVER_NAME}")
                        } else {
                            echo "SSH key successfully distributed to ${params.SERVER_NAME}"
                        }
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
