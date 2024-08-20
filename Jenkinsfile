pipeline {
    agent { label 'deploy' }

    stages {
        stage('Fetching update') {
            steps {
                sh '''
                rm -rf s7rosine-do-it-yourself-devops-automation || true
                rm -rf ~/deployment/* || true
                git clone git@github.com:DEL-ORG/s7rosine-do-it-yourself-devops-automation.git ~/deployment
                '''
            }
        }

        stage('Pull image') {
            steps {
                sh '''
                cd ~/deployment
                docker-compose pull
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cd ~/deployment
                docker-compose up -d --remove-orphans
                '''
            }
        }

        stage('List containers') {
            steps {
                sh '''
                cd ~/deployment
                docker-compose ps
                '''
            }
        }
    }
}
