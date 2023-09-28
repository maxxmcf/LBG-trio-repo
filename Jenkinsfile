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
                docker stop engine
                docker remove engine
                docker stop flask-app
                docker remove flask-app
                docker stop mysql
                docker remove mysql
                docker run -d -p 80:80 --name engine maxmcf13/mynginx:latest
                docker run -d --name flask-app maxmcf13/myapp:latest
                docker run -d --name mysql maxmcf13/mydb:latest
                docker network create mynw
                docker network connect mynw myengine
                docker network connect mynw flask-app
                docker network connect mynw mysql
                docker run -d -p 80:80 --name engine maxmcf13/mynginx:latest
                '''
            }
        }
    }
}