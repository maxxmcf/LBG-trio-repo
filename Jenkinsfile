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
                docker push mynginx:latest
                docker push myapp:latest
                docker push mydb:latest
                '''
            }
        }
        stage('running app') {
            steps {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.28 << EOF
                docker pull mynginx:latest
                docker pull myapp:latest
                docker pull mydb:latest
                docker run -d -p 80:80 mynginx:latest
                docker run -d --name flask-app myapp:latest
                docker run -d --name flask-db mydb:latest
                '''
            }
        }
    }
}