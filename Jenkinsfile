pipeline {
    agent any
    stages {
        stage ('Verify Tooling') {
            steps {
                sh'''
                docker version
                docker info
                docker compose version
                '''
            }
        }
        stage ('Checkout repo') {
            dir('idk') {
                checkout scm
            }
        }
        stage ('Remove old containers') {
            steps {
                sh'docker compose down --remove-orphans -v'
            }
        }
        stage ('Docker Compose Test') {
            steps {
                sh'docker compose up -d --no-color'
                sh'docker ps'
            }
        }
        stage ('Test') {
            steps {
                sh'docker exec zap zap-baseline.py -t http://webgoat:8080/WebGoat/ -n data/WebGoat.context'
            }
        }
        stage ('Clean up') {
            steps {
                sh'docker compose down --remove-orphans -v'
            }
        }  
    }
}
