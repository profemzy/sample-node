pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/sampleNode.zip'
            }
        }

         stage('Building Docker Image') {
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
                    steps{
                      script {
                         docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                                  app.push("${env.BUILD_NUMBER}")
                                  app.push("latest")
                         }
                      }
                    }
          }
    }
}