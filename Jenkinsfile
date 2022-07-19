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
        stage ('Docker Compose Test') {
            steps {
                sh'docker compose up -d --no-color'
                sh'docker ps'
            }
        }
        stage ('Zap Status') {
            steps {
                sh'docker exec zap zap-cli --api-key 5364864132243598723485 status '
            }
        }
        stage ('Test') {
            steps {
                sh'docker exec zap zap-baseline.py -t http://localhost:8081/WebGoat'
            }
        }
        stage ('Remove Containers') {
            steps {
                sh'docker compose down --remove-orphans -v'
            }
        }
    }
}
