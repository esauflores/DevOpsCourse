pipeline {
    agent any

    tools {
       go "1.24.1"
    }

    stages {
        // stage('Setup Go Module') {
        //     steps {
        //         sh """
        //             if [ ! -f go.mod ]; then
        //                 go mod init github.com/esauflores/DevOpsCourse
        //             fi
        //             go mod tidy
        //         """
        //     }
        // }
     
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
    }
}
