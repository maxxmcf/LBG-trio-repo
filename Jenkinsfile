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
                docker build -t maxmcf13/mynginx:latest ./nginx
                docker build -t maxmcf13/myapp:latest ./flask-app
                docker build -t maxmcf13/mydb:latest ./db
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
                docker pull maxmcf13/mynginx:latest
                docker pull maxmcf13/myapp:latest
                docker pull maxmcf13/mydb:latest
                docker run -d -p 80:80 mynginx:latest
                docker run -d --name flask-app myapp:latest
                docker run -d --name flask-db mydb:latest
                '''
            }
        }
    }
}