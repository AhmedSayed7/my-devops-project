pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhubCred')
        IMAGE_NAME = 'ahmedsayed7/flask-app'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/AhmedSayed7/my-devops-project.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }
        stage('Push to Docker Hub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'helm upgrade --install flask-app ./charts --set image.repository=$IMAGE_NAME --set image.tag=$BUILD_NUMBER'
            }
        }
    }
}