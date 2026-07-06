pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh 'docker build -t website:v1 .'
            }
        }

        stage('Test') {
            steps {
                sh '''
                docker rm -f website-test || true
                docker run -d --name website-test -p 8081:80 website:v1
                sleep 5
                curl http://localhost:8081
                docker rm -f website-test
                '''
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }
            steps {
                echo 'Deploying to Production Server'
            }
        }
    }
}
