pipeline {
    agent any
    environment {
        BASE_PATH = 'C:/infra/minha-cloud'
        KUBECONFIG = 'C:/Users/profl/.kube/config'
    }
    stages {
        stage('Build Docker Image') {
            steps {
                bat "docker build -t node-simples:%BUILD_ID% -f %BASE_PATH%/Dockerfile %BASE_PATH%"
            }
        }
        stage('Load Image into Kind') {
            steps {
                bat 'kind load docker-image node-simples:%BUILD_ID% --name kind'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {

                bat """
                powershell -Command "(Get-Content %BASE_PATH%/deployment.yaml) -replace '{{tag}}', '%BUILD_ID%' | Set-Content %BASE_PATH%/new-deployment.yaml"
                """

                bat 'kubectl config use-context kind-kind'
                bat "kubectl apply -f %BASE_PATH%/namespace.yaml --validate=false"
                bat "kubectl apply -f %BASE_PATH%/new-deployment.yaml --validate=false"
                //bat 'kubectl --kubeconfig=%KUBECONFIG% apply -f %BASE_PATH%/deployment.yaml'
                bat "kubectl apply -f %BASE_PATH%/service.yaml --validate=false"
            }
        }
    }
}