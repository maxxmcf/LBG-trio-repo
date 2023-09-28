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
        stage('build docker images') {
            steps {
                sh '''
                docker build -t maxmcf13/mynginx:latest ./nginx
                docker build -t maxmcf13/mynginx:${BUILD_NUMBER} ./nginx
                docker build -t maxmcf13/myapp:latest ./flask-app
                docker build -t maxmcf13/myapp:${BUILD_NUMBER} ./flask-app
                docker build -t maxmcf13/mydb:latest ./db
                docker build -t maxmcf13/mydb:${BUILD_NUMBER} ./db
                '''
            }
        }
        stage('push docker images') {
            steps {
                sh '''
                docker push maxmcf13/mynginx:latest
                docker push maxmcf13/mynginx:${BUILD_NUMBER}
                docker push maxmcf13/myapp:latest
                docker push maxmcf13/myapp:${BUILD_NUMBER}
                docker push maxmcf13/mydb:latest
                docker push maxmcf13/mydb:${BUILD_NUMBER}
                '''
            }
        }
        stage('clean jenky') {
            steps {
                sh '''
                docker rmi maxmcf13/mynginx:latest
                docker rmi maxmcf13/mynginx:${BUILD_NUMBER}
                docker rmi maxmcf13/myapp:latest
                docker rmi maxmcf13/myapp:${BUILD_NUMBER}
                docker rmi maxmcf13/mydb:latest
                docker rmi maxmcf13/mydb:${BUILD_NUMBER}
                '''
            }
        }
        stage('running app') {
            steps {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.28 << EOF
                docker pull maxmcf13/mynginx
                docker pull maxmcf13/myapp
                docker pull maxmcf13/mydb
                docker network inspect mynw && sleep 1 || docker network create mynw
                docker volume inspect mynw && sleep 1 || docker volume create mynw
                if [ docker stop mysql ]; then
                  docker rm mysql
                else
                  if [ docker rm mysql ]; then
                    sleep 1
                  else
                    sleep
                  fi
                fi
                if [ docker stop engine ]; then
                  docker rm engine
                else
                  if [ docker rm engine ]; then
                    sleep 1
                  else
                    sleep
                  fi
                fi
                if [ docker stop flask-app ]; then
                  docker rm flask-app
                else
                  if [ docker rm flask-app ]; then
                    sleep 1
                  else
                    sleep
                  fi
                fi
                docker run -d -p 80:80 --network mynw --name engine maxmcf13/mynginx
                docker run -d --network mynw --name flask-app maxmcf13/myapp
                docker run -d -v mynw:/var/lib/mysql --network mynw --name mysql maxmcf13/mydb
                '''
            }
        }
    }
}