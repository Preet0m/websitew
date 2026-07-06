pipeline {
    agent any

    environment {
        IMAGE_NAME = "website:v1"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''
                docker build -t $IMAGE_NAME .
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                docker rm -f website-test || true

                docker run -d --name website-test -p 8081:80 $IMAGE_NAME

                sleep 10

                docker ps -a

                docker logs website-test || true

                docker rm -f website-test || true
                '''
            }
        }

        stage('Deploy') {
            when {
                branch 'master'
            }

            steps {
                sh '''
                docker rm -f website-container || true

                docker run -d \
                --name website-container \
                -p 80:80 \
                $IMAGE_NAME
                '''
            }
        }
    }

    post {

        success {
            echo 'Pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed.'
        }

        always {
            sh 'docker image ls'
        }
    }
}
