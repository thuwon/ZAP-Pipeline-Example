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
                 sh'docker cp ./zap/data/WebGoat.context zap:zap/wrk/data'
            }
        }
        stage ('Build Seleniumbase image') {
            steps {
                sh'ls -a'
                sh'docker build -t seleniumbase -f ./selenium/Dockerfile .'
                sh'docker run -p 4444:4444 seleniumbase'
                sh'sleep 3s'
                sh'docker ps'
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
