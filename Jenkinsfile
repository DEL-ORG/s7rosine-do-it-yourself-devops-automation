pipeline {
    agent any

    stages {
        stage('Scan Golang Code') {
            agent {
                docker {
                    image 'golang:1.22.5'
                    args '-u root' // Run the container as the root user
                }
            }
            steps {
                sh '''
                cd do-it-yourself/src/catalog/
                ls -la 
                whoami
                go test
                '''
            }
        }
        
        