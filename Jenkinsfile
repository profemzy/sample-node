pipeline {
   environment {
     DOCKER_IMAGE_NAME = "profemzy/docker-nodejs"
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
         echo 'Running build automation'
         sh 'npm install'
       }
    }
    stage('Test App') {
      steps {
        sh 'npm test'
      }
    }
    stage('Building Docker Image') {
           when {
                     branch 'master'
           }
           steps{
             script {
               app = docker.build(DOCKER_IMAGE_NAME)
               app.inside {
                       sh 'echo Hello, World!'
                }
             }
           }
     }
     stage('Push Docker Image') {
            when {
                branch 'master'
           }
           steps{
             script {
                docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                         app.push("${env.BUILD_NUMBER}")
                         app.push("latest")
                }
             }
           }
      }

     stage('Remove Unused Docker Image') {
             steps{
               sh "docker rmi ${DOCKER_IMAGE_NAME}:${env.BUILD_NUMBER}"
            }
     }
  }
}