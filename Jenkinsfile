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
                docker build + run
                '''
            }
        }
        stage('running app') {
            steps {
                sh '''
                ssh -i "~/.ssh/id_rsa" jenkins@10.154.0.28 << EOF
                rm -rf freestyle-demo
                git clone git@github.com:maxxmcf/freestyle-demo.git
                cd freestyle-demo
                echo "hi there - you are on the SECOND server"
                chmod +x hello.sh
                ./hello.sh
                hostname
                '''
            }
        }
    }
}