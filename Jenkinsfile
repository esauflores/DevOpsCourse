pipeline {
    agent any

    tools {
       go "1.24.1"
    }


    environment {
        IMAGE_NAME = "ttl.sh/esauflores-devops-go-app:2h"
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
                sshagent(['target-ssh-credentials']) {
                    sh '''
                    mkdir -p ~/.ssh
                    ssh-keyscan -H target >> ~/.ssh/known_hosts
                    ansible-playbook --inventory hosts.ini playbook.yml --extra-vars "image_name=${IMAGE_NAME}"
                    '''
                }
            }
        }
    }
}
