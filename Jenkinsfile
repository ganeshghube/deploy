pipeline {

  agent any
  stages {
    stage('Cloning Gitt') {
      steps {
        sh "rm -rf *"
        sh "git clone https://github.com/ganeshghube/deploy.git"
      }
    }
  }
}