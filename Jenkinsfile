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
    stage('Building image') {
           steps{
             script {
               dockerImage = docker.build dockerRegistry + ":$BUILD_NUMBER"
             }
           }
     }
     stage('Upload Image') {
           steps{
             script {
               docker.withRegistry( '', dockerRegistryCredential ) {
                 dockerImage.push()
               }
             }
           }
      }

     stage('Remove Unused docker image') {
             steps{
               sh "docker rmi $dockerRegistry:$BUILD_NUMBER"
            }
     }
  }
}