pipeline {
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Repository') {
      steps {
        git 'https://github.com/profemzy/sample-node'
      }
    }
    stage('Build') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test') {
      steps {
        sh 'npm test'
      }
    }
  }
}