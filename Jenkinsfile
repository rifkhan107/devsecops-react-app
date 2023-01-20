pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/rifkhan107/devsecops-react-app.git', branch: 'master'
            }
        }
        stage('Build') {
            steps {
                sh 'sudo npm install'
                sh 'sudo npm run build'
            }
        }
       stages {
        stage('Deploy') {
            steps {
		    sh 'sudo rm -rf /var/www/jenkins-react-app'
                withCredentials([sshUserPrivateKey(credentialsId: 'my-ssh-key', keyFileVariable: 'SSH_KEY_FILE')]) {
                    sh "sudo rsync -avz ${WORKSPACE}/build/ /var/www/jenkins-react-app/ -e 'ssh -i ${SSH_KEY_FILE}'"
            }
        }
    }
}

        stage('Security Testing') {
            steps {
                sh 'owasp-zap-cli quick-scan --start-options "-config api.disablekey=true" --spider http://20.204.141.43:3000/my-react-app'
            }
        }
    }
}

