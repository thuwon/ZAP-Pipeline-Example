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
        //stage ('Remove old containers') {
        //    steps {
        //        sh'docker stop sad_banzai'
        //        sh'docker rm sad_banzai'
        //        sh'docker compose down --remove-orphans -v'
        //        sh'docker network rm testnet'
        //    }
        //}
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
                sh'docker run --name sad_banzai -d -p 4444:4444 -itd --network=dockercompose_default seleniumbase'
            }
        }
        stage('Create test network') {
            steps {
                sh'sleep 15s'
                sh'docker network create --subnet 172.28.0.0/16 testnet'
                sh'docker network connect --ip 172.28.1.1 testnet zap'
                sh'docker network connect --ip 172.28.1.2 testnet goat'
                sh'docker network connect --ip 172.28.1.3 testnet sad_banzai'
                
            }
        }
        stage('Test using seleniumbase') {
            steps {
                sh'docker exec sad_banzai py.test -s --browser=chrome --headless --proxy=172.28.1.1:8091'
                sh'docker exec sad_banzai curl -X GET http://zap:8091/HTML/core/view/alerts > /results/zap/html/results.HTML'
                sh'docker exec sad_banzai curl -X GET http://zap:8091/JSON/core/view/alerts > /results/zap/json/results.JSON'
                sh'docker exec sad_banzai cat /results/zap/json/results.JSON'
            }
        }
        stage ('Test') {
            steps {
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
