pipeline {
    agent any
    stages {
        stage('Clone') {
            steps {
                git url: 'https://github.com/rifkhan107/devsecops-react-app.git', branch: 'main'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
                sh 'npm test'
                if (currentBuild.result == 'FAILURE') {
                    error "Tests failed"
                }
                sh 'npm run build'
            }
        }
        stage('Deploy') {
            steps {
                sh 'sudo rm -rf /var/www/jenkins-react-app'
                sh 'sudo rsync -avz ${WORKSPACE}/build/ /var/www/jenkins-react-app/'
            }
        }

        stage('Security Testing') {
            steps {
                sh 'owasp-zap-cli quick-scan --start-options "-config api.disablekey=true" --spider http://20.204.141.43:3000/my-react-app'
            }
        }
    }
}
