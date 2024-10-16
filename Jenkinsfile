pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                    def app = docker.build("my-node-app:${env.BUILD_ID}")
                }
            }
        }
        stage('Push') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerhub-credentials') {
                        app.push("${env.BUILD_ID}")
                        app.push("latest")
                    }
                }
            }
        }
        stage('Deploy Node.js') {
            steps {
                sh 'kubectl apply -f k8s/nodejs-deployment.yaml'
                sh 'kubectl apply -f k8s/nodejs-service.yaml'
            }
        }
        stage('Deploy MySQL') {
            steps {
                sh 'kubectl apply -f k8s/mysql/mysql-deployment.yaml'
                sh 'kubectl apply -f k8s/mysql/mysql-service.yaml'
            }
        }
    }
}
