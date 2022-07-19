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
        stage ('Open Webswing') {
            steps {
                sh'docker run -u zap -p 8080:8080 -p 8090:8090 -d --name zap --hostname zap owasp/zap2docker-stable zap-webswing.sh --wait'
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
