pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo = "copying all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* ubuntu@3.137.185.142:/home/ubuntu"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh 'scp $keyfile ubuntu@3.137.185.142:/home/ubuntu/.ssh/WinDevOps2020.pem'
                        }
                    }
                }
            }
        }
        stage ("Execute Ansible Playbook") {
            steps{
                script{
                    echo "Calling Ansible Playbook to configure ec2 instances"

                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "3.137.185.142"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                        remote.user = user
                        remote.identityFile = keyfile
                        sshCommand remote: remote, command: "ansible-playbook my-playbook.yaml"
                    }
                }
            }
        }
    }   
}