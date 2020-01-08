pipeline {
    environment {
//         DOCKER_IMAGE_NAME = "profemzy/docker-nodejs"
        DOCKER_IMAGE_NAME = "686233958969.dkr.ecr.eu-west-1.amazonaws.com/sample-node"
      }
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
                          docker.withRegistry("https://686233958969.dkr.ecr.eu-west-1.amazonaws.com", "aws_ecr_login") {
                                  app.push("${env.BUILD_NUMBER}")
                                  app.push("latest")
                         }
                      }
                    }
          }
    }
}