pipeline {
   environment {
     dockerRegistry = "profemzy/docker-nodejs"
     dockerRegistryCredential = 'dockerhub'
     dockerImage = ''
   }
  agent any
  tools {nodejs "node" }
  stages {
    stage('Cloning Repository') {
      steps {
        git 'https://github.com/profemzy/sample-node'
      }
    }
    stage('Build App') {
       steps {
         sh 'npm install'
       }
    }
    stage('Test App') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building Docker Image') {
           steps{
             script {
               dockerImage = docker.build dockerRegistry + ":$BUILD_NUMBER"
             }
           }
     }
     stage('Upload Docker Image') {
           steps{
             script {
               docker.withRegistry( '', dockerRegistryCredential ) {
                 dockerImage.push()
               }
             }
           }
      }

     stage('Remove Unused Docker Image') {
             steps{
               sh "docker rmi $dockerRegistry:$BUILD_NUMBER"
            }
     }
  }
}