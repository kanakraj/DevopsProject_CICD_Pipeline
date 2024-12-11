pipeline {
    agent {
        label "slave"
    }
    stages {
        stage("Cloning") {
            steps {
                echo "Cloning repo to the server"
                // Change the branch to 'developer'
                git url: "https://github.com/kanakraj/DevopsProject_CICD_Pipeline.git", branch: "developer"
            }
        }
        stage("Building image") {
            steps {
                echo "Building image through Dockerfile"
                sh "sudo chmod 777 /var/run/docker.sock"
                sh "docker build -t kanakraj321/coffeeshop ."
            }
        }
        stage("Pushing on DockerHub") {
            steps {
                echo "Pushing this image on DockerHub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "dockeruser", passwordVariable: "dockerpass")]) {
                    sh "docker login -u ${env.dockeruser} -p ${env.dockerpass}"
                    sh "docker push kanakraj321/coffeeshop"
                }
            }
        }
        stage("Running container") {
            steps {
                echo "Running coffeeshop on container"
                sh "docker run -itd -p 80:80 kanakraj321/coffeeshop"
            }
        }
    }
}
