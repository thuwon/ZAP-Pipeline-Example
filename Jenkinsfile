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
        stage ('Cat?') {
            steps {
                sh'cat ./zap/data/WebGoat.context'
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
        stage ('Add Webgoat Context') {
            steps {
                sh'curl -LJO https://raw.githubusercontent.com/thuwon/trigger/main/zap/data/WebGoat.context'
                sh'docker cp ./WebGoat.context zap:zap/wrk/data'
            }
        }
        stage ('Test') {
            steps {
                sh'sleep 15s'
                sh'docker exec zap zap-full-scan.py -t http://webgoat:8080/WebGoat/ -n data/WebGoat.context'
            }
        }
        stage ('Clean up') {
            steps {
                sh'docker compose down --remove-orphans -v'
            }
        }  
    }
}
