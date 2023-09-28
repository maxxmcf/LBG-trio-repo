pipeline {
    agent any
    stages {
        stage('welcome') {
            steps {
                sh '''
                echo "hi there - welcome to the pipeline"
                '''
            }
        }
        stage('doing docker') {
            steps {
                sh '''
                docker build -t mynginx:latest ./nginx
                docker build -t myapp:latest ./flask-app
                docker build -t mydb:latest ./db
                docker push maxmcf13/mynginx:latest
                docker push maxmcf13/myapp:latest
                docker push maxmcf13/mydb:latest
                '''
            }
        }
        stage('running app') {
            steps {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.28 << EOF
                docker pull x3
                docker run x3
                network???
                '''
            }
        }
    }
}