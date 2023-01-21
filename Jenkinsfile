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
                withCredentials([sshUserPrivateKey(credentialsId: 'root-key', keyFileVariable: 'PRIVATE_KEY')]) {
                sh 'sudo rm -rf /var/www/jenkins-react-app' 
                sh 'rsync -avz ${WORKSPACE}/build/ /var/www/jenkins-react-app/'
                }
            }
         }

        stage('Security Testing') {
            steps {
                sh 'zap-cli quick-scan --start-options "-config api.disablekey=true" --spider https://techventuras.com/'
                publishHTML target: [allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: '.', reportFiles: 'zap-report.html', reportName: 'ZAP Report']
            }
         }
    }
}


