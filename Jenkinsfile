pipeline {
    agent any
    stages {
        stage('on jenkins server') {
            steps {
                sh '''
                echo "hi there - you are on the JENKINS server"
                chmod +x hello.sh
                ./hello.sh
                hostname
                '''
            }
        }
        stage('on 2nd server') {
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