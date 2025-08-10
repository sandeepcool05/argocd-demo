pipeline {
    agent any

    stages {
    stage ('Clean Workspace') {
        steps {
            sh 'rm -rf argocd-demo'
        }
    }
        stage('Clone Repo') {
            steps {
                script {
                sh 'git -c http.sslVerify=false clone https://github.com/sandeepcool05/argocd-demo.git' 
            }
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
                sh 'echo $DOCKER_PASS | docker login -u scool05 --password-stdin'
                sh 'docker tag myapp:latest scool05/myapp:latest'
                sh 'docker push scool05/myapp:latest'
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
