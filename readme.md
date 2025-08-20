kubectl apply -f namespace.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

1 - kubectl get namespaces 
2 - kubectl get deployments -n meu-cloud-node
3 - kubectl get pods -n meu-cloud-node

# Com namespace 
kubectl apply -f deployment.yaml -n meu-cloud-node
kubectl apply -f service.yaml -n meu-cloud-node


# Apagando 
## Deleta tudo 
kubectl delete namespace meu-cloud-node
kubectl delete all --all -n meu-cloud-node

## Deleta o namespace
kubectl create namespace meu-cloud-node

## Deletar e criar o cluster
kind delete cluster --name meu-cluster
kind create cluster --name meu-cluster

## Pegar todos clusters
kind get clusters
kind load docker-image node-simples:1.0 --name kind 

## Verificar no docker
docker images | grep node-simples
docker run -ti --name my-node-1 -p 93:3004 node-simples:1.0

## Passos 
1 - Criar Dockerfile
2 - Criar executar docker build  (docker build -t node-simples:1.0 .)
3 - Criar executar docker run para testar ( docker run -ti --name my-node-1 -p 93:3004 node-simples:1.0 )
4 - Crregar imagem para o kind (kind load docker-image node-simples:1.0 --name kind )
5 - kubectl apply -f namespace.yaml
6 - kubectl apply -f deployment.yaml
7 - kubectl apply -f service.yaml

## Verificando Pods 
kubectl delete pod -l app=node-simples -n meu-cloud-node
kubectl apply -f deployment.yaml -n meu-cloud-node
kubectl get pods -n meu-cloud-node
kubectl describe pod <nome-do-pod> -n meu-cloud-node
kubectl describe pod node-simples-58bd47c98b-fdmfm -n meu-cloud-node

## Automatizando com o jenkins
docker run -d --name jenkins --network host -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
já instalado na maquina: http://localhost:8081/

## Jenkinsfile
pipeline {
    agent any
    stages {
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t node-simples:1.0 ./app'
            }
        }
        stage('Load Image into Kind') {
            steps {
                sh 'kind load docker-image node-simples:1.0 --name meu-cluster'
            }
        }
        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f k8s/deployment.yaml'
                sh 'kubectl apply -f k8s/service.yaml'
            }
        }
    }
}

## Como ver se é um load balancer
kubectl get svc -n meu-cloud-node
kubectl describe svc node-simples-service -n meu-cloud-node

## Tentar alterar o contexto 
kubectl config use-context kind-kind
kubectl config get-contexts
kind create cluster --name kind

## Configurações do cluster
bat 'kubectl config use-context kind-kind'
KUBECONFIG = 'C:/Users/profl/.kube/config'

## Deploy automatico github
https://4c20440ef14f.ngrok-free.app/github-webhook/