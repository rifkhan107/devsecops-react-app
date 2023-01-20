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
        stage('Deploy') {
            steps {
                sh 'sudo rm -rf /var/www/jenkins-react-app'
                sh 'scp -i /tmp/ubuntu-devsecops-key.pem -r ${WORKSPACE}/build/ azureuser@20.204.141.43:/var/www/jenkins-react-app/'
            }
        }

        stage('Security Testing') {
            steps {
                sh 'owasp-zap-cli quick-scan --start-options "-config api.disablekey=true" --spider http://20.204.141.43:3000/my-react-app'
            }
        }
    }
}
