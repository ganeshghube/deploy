# Jenkins with GitHub Integration Pipeline Example
pipeline {
  agent any
  stages {
    stage('GitHub checkout') {
      steps {
        git 'https://github.com/learn-devops-fast/rps-ant.git'
        sh 'ant clean compile test package war'
      }
    }
  }
}