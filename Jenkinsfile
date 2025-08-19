pipeline {
    agent any
    environment {
        BASE_PATH = 'C:/infra/minha-cloud'
        KUBECONFIG = 'C:/Users/profl/.kube/config'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t node-simples:1.0 -f %BASE_PATH%/Dockerfile %BASE_PATH%"
            }
        }
        stage('Load Image into Kind') {
            steps {
                bat 'kind load docker-image node-simples:1.0 --name kind'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                bat 'kubectl config use-context kind-kind'
                bat "kubectl apply -f %BASE_PATH%/namespace.yaml --validate=false"
                bat "kubectl apply -f %BASE_PATH%/deployment.yaml --validate=false"
                bat "kubectl apply -f %BASE_PATH%/service.yaml --validate=false"
            }
        }
    }
}