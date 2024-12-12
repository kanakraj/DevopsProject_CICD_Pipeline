pipeline {
    agent any
    stages {
        stage('Build and run docker image') {
            steps {
                sh 'docker pull kanakraj321/coffeeshop'
                sh 'docker run -d -p 84:80 kanakraj321/coffeeshop'
            } 
        }


        stage('testing') {
            steps {
                sh 'curl -I 3.99.183.21:84'
            }
        }

    
    }
}
