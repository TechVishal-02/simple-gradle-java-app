pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        AWS_CREDENTIALS = credentials('aws-credentials')
        IMAGE_NAME = 'techvishal02/simple-gradle-java-app'
    }
    stages {
        stage('Checkout Code') {
            steps {
                git 'https://github.com/TechVishal-02/simple-gradle-java-app.git'
            }
        }
        stage('Build and Test') {
            steps {
                sh './gradlew build'
                sh './gradlew test'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t $IMAGE_NAME ."
            }
        }
        stage('Push Image to DockerHub') {
            steps {
                sh "echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin"
                sh "docker push $IMAGE_NAME"
            }
        }
        stage('Deploy to EKS') {
            steps {
                sh '''
                aws eks --region us-east-1 update-kubeconfig --name my-cluster
                kubectl apply -f k8s/deployment.yaml
                '''
            }
        }
    }
}
