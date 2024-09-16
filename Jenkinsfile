pipeline {

  agent any
  stages {
    stage('Cloning Git') {
      steps {
        sh "rm -rf *"
        sh "git clone https://github.com/ganeshghube/deploy.git"
      }
    }
  }
}