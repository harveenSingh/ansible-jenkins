pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo = "copyihng all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@3.16.68.133:/root"

                        withCredentials([sshUserPrivateKey(credentialsID: "ansible-server-key", keyFileVariable: 'keyfile')])
                            sh "scp ${keyfile} root@3.137.185.142:/root/WinDevOps2020.pem"
                    }
                }
            }
        }   
    }   
}