pipeline {
    agent any

    tools {
       go "1.24.1"
    }


    environment {
        IMAGE_NAME = "ttl.sh/esauflores-devops-go-app:2h"
        KUBERNETES_SERVER_URL = "https://kubernetes:6443"
    }

    stages {
        stage('Test') {
            steps {
                sh "go test ./..."
            }
        }

        stage('Build') {
            steps {
                sh "go build main.go"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME} ."
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker push ${IMAGE_NAME}"
            }
        }

        stage('Deploy') {
            steps {
                withKubeConfig(
                    credentialsId: 'k8s-jenkins-token',
                    serverUrl: KUBERNETES_SERVER_URL,
                    insecureSkipTlsVerify: true
                ) {
                    sh "kubectl apply -f definition.yaml"
                }
            }
        } 
    }
}
