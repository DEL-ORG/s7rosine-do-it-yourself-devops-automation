pipeline {
    agent any

    stages {
        stage('clean up env') {
            steps {
                sh '''
                cd ~/automation s7rosine-do-it-yourself-devops-automation
                docker-compose down --remove-orphans
                '''
            }
        }

        stage('pull image') {
            steps {
                sh '''
                cd ~/automation s7rosine-do-it-yourself-devops-automation
                docker-compose pull
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                cd ~/automation s7rosine-do-it-yourself-devops-automation
                docker-compose up -d --remove-orphans
                '''
            }
        }
    }
}        stage('Deploy') {
            steps {
                sh '''
                cd ~/automation s7rosine-do-it-yourself-devops-automation
                docker-compose up -d --remove-orphans
                '''
            }
        }
}        stage('list containers') {
            steps {
                sh '''
                cd ~/automation s7rosine-do-it-yourself-devops-automation
                docker-compose ps
                '''
            }
        }