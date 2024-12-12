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
        sshagent(['staging_server']) {
          withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "dockeruser", passwordVariable: "dockerpass")]) {
            sh "ssh root@3.97.145.24 \"docker login -u ${dockeruser} -p ${dockerpass}\""
            sh "ssh root@3.97.145.24 \"docker pull kanakraj321/coffeeshop\""
          }
        }
      }
    }

    stage("Deploying Container on Staging Server") {
      steps {
        echo "Deploying the container on the staging server"
        sshagent(['staging_server']) {
          sh "ssh root@3.97.145.24 \"docker stop staging_container || true\""
          sh "ssh root@3.97.145.24 \"docker rm staging_container || true\""
          sh "ssh root@3.97.145.24 \"docker run -d --name staging_container -p 80:80 kanakraj321/coffeeshop\""
        }
      }
    }

    stage('Test Website') {
      steps {
        echo "Testing if the website is accessible on the staging server"
        sshagent(['staging_server']) {
          sh "ssh root@3.97.145.24 \"curl -I http://3.97.145.24:80 || exit 1\""
        }
      }
    }
  }
}
