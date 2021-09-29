pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo = "copyihng all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* ubuntu@3.137.185.142:/home/ubuntu"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh 'scp $keyfile ubuntu@3.137.185.142:/home/ubuntu/WinDevOps2020.pem'
                        }
                    }
                }
            }
        }   
    }   
}