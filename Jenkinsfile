pipeline {
    agent any
    stages {
        stage('Cleanup') {
            steps {
                cleanWs() 
            }
        }

        stage('Checkout Code') {
            steps {
                checkout scm 
            }
        }

        stage("Building Image") {
            steps {
                echo "Building image through Dockerfile"
                sh "docker build -t kanakraj321/coffeeshop ."
            }
        }

        stage("Running Container") {
            steps {
                echo "Running coffeeshop on container"
                sh "docker stop develop_container || true"
                sh "docker rm develop_container || true"
                sh "docker run -d --name develop_container -p 80:80 kanakraj321/coffeeshop"
            }
        }

        stage('Test Website') {
            steps {
                echo "Testing if the website is accessible on the container"
                sh "curl -I http://15.222.200.200:80 || exit 1"
            }
        }

        stage("Pushing to DockerHub") {
            steps {
                echo "Pushing this image to DockerHub"
                withCredentials([usernamePassword(credentialsId: "dockerhub", usernameVariable: "dockeruser", passwordVariable: "dockerpass")]) {
                    sh "docker login -u ${env.dockeruser} -p ${env.dockerpass}"
                    sh "docker push kanakraj321/coffeeshop"
                }
            }
        }
    }
}
