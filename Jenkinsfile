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
        sh 'docker run --rm website:v1 ls /var/www/html'
    }
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
