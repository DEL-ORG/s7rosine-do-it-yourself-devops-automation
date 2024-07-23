pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('del-docker-hub-auth')
        CI = 'true'
       
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '20'))
        disableConcurrentBuilds()
        timeout(time: 60, unit: 'MINUTES')
        timestamps()
    }

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
                cd catalog/
                go test
                '''
            }
        }

        stage('SonarQube analysis') {
            agent {
                docker {
                    image 'sonarsource/sonar-scanner-cli:5.0.1'
                }
            }
            environment {
            CI = 'true'
            scannerHome='/opt/sonar-scanner'
            }
            steps {
                withSonarQubeEnv('Sonar') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }

        stage('Login') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('build-image-api') {
            steps {
                sh '''
                cd ${WORKSPACE}/catalog
                docker build -t devopseasylearning/s7Rosine-do-it-yourself-catalog-api:${BUILD_NUMBER} .
                '''
            }
        }

        stage('build-image-db') {
            steps {
                sh '''
                cd ${WORKSPACE}/catalog
                docker build -t devopseasylearning/s7Rosine-do-it-yourself-catalog-db:${BUILD_NUMBER} . -f Dockerfile-db
                '''
            }
        }
    }

    post {
        success {
            slackSend(channel: '#development-alerts', color: 'good', message: "SUCCESSFUL: Application s7Rosine-do-it-yourself-catalog Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        unstable {
            slackSend(channel: '#development-alerts', color: 'warning', message: "UNSTABLE: Application s7Rosine-do-it-yourself-catalog Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        failure {
            slackSend(channel: '#development-alerts', color: '#FF0000', message: "FAILURE: Application s7Rosine-do-it-yourself-catalog Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
        }
        cleanup {
            deleteDir()
        }
    }
}
