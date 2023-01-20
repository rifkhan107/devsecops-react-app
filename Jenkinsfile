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
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: '<your_credentials_id>', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                sh "sudo -u ${USERNAME} -i -- sh -c 'rm -rf /var/www/jenkins-react-app'"
                sh "sudo -u ${USERNAME} -i -- sh -c 'rsync -avz ${WORKSPACE}/build/ /var/www/jenkins-react-app/'"
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

