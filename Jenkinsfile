pipeline {
  agent any
  stages {
    stage('Cleanup') {
      steps {
        cleanWs() 
      }
    }

    stage("Pulling Image from DockerHub") {
      steps {
        echo "Pulling Docker image from DockerHub"
        sshagent(['production_server']) {
          withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "dockeruser", passwordVariable: "dockerpass")]) {
            sh "ssh root@35.182.168.187 \"docker login -u ${dockeruser} -p ${dockerpass}\""
            sh "ssh root@35.182.168.187 \"docker pull kanakraj321/coffeeshop\""
          }
        }
      }
    }

    stage("Deploying Container on Production Server") {
      steps {
        echo "Deploying the container on the production server"
        sshagent(['production_server']) {
          sh "ssh root@35.182.168.187 \"docker stop production_container || true\""
          sh "ssh root@35.182.168.187 \"docker rm production_container || true\""
          sh "ssh root@35.182.168.187 \"docker run -d --name production_container -p 80:80 kanakraj321/coffeeshop\""
        }
      }
    }

    stage('Test Website') {
      steps {
        echo "Testing if the website is accessible on the production server"
        sshagent(['production_server']) {
          sh "ssh root@35.182.168.187 \"curl -I http://35.182.168.187:80 || exit 1\""
        }
      }
    }
  }
}
