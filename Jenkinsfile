pipeline {
    agent any
    environment {
        ANSIBLE_SERVER = "3.137.159.32"
    }
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo = "copying all necessary files to ansible control node"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* ubuntu@${ANSIBLE_SERVER}:/home/ubuntu"

                        withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                            sh 'scp $keyfile ubuntu@$ANSIBLE_SERVER:/home/ubuntu/.ssh/WinDevOps2020.pem'
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
                    remote.host = ANSIBLE_SERVER
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: 'ansible-server-key', keyFileVariable: 'keyfile', usernameVariable: 'user')]){
                        remote.user = user
                        remote.identityFile = keyfile
                        sshScript remote: remote, script: "prepare-ansible-server.sh"
                        sshCommand remote: remote, command: "ansible-playbook my-playbook.yaml"
                    }
                }
            }
        }
    }   
}