pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'master', url: 'https://github.com/sandeepcool05/argocd-demo.git'
            }
        }
    stage('Build Docker Image') {
            steps {
                sh 'docker build -t myapp:latest .'
            }
        }
    stage('Push to Docker Hub') {
            steps {
              withCredentials([string(credentialsId: 'dockerhub-token', variable: 'DOCKER_PASS')]) {
                sh 'echo $DOCKER_PASS | docker login -u yourdockerhubusername --password-stdin'
                sh 'docker tag myapp:latest yourdockerhubusername/myapp:latest'
                sh 'docker push yourdockerhubusername/myapp:latest'
              }
            }
        }        
    stage('Deploy to Kubernetes via kubectl') {
            steps {
              sh 'kubectl apply -f k8s/'
            }
        }
    }
}                    
