pipeline {
    agent any

    environment {
        IMAGE_NAME = "react-app"
    }

    stages {
        stage('Build React App') {
            steps {
                sh 'npm install'
                sh 'npm run build'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }

        stage('Run Docker Container') {
            steps {
                sh """
                   docker stop $IMAGE_NAME || true
                   docker rm $IMAGE_NAME || true
                   docker run -d -p 3000:80 --name $IMAGE_NAME $IMAGE_NAME
                """
            }
        }
    }
}
