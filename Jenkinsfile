pipeline {
    agent any

    tools {
       go "1.24.1"
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

        stage('Prepare SSH') {
            steps {
                sh '''
                  mkdir -p ~/.ssh
                  chmod 700 ~/.ssh
                  ssh-keyscan -H target >> ~/.ssh/known_hosts
                  chmod 600 ~/.ssh/known_hosts
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh 'scp main laborant@target:~'
            }
        }

    }
}
