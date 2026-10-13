pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                echo 'Getting code from GitHub...'
                checkout scm
            }
        }

        stage('Test') {
            steps {
                echo 'Checking index.html...'
                bat 'if not exist index.html exit 1'
                bat 'findstr /i "<html" index.html'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying index.html...'
                bat 'if not exist C:\\deploy mkdir C:\\deploy'
                bat 'copy index.html C:\\deploy\\index.html /Y'
            }
        }
    }

    post {
        success { echo 'Pipeline finished successfully!' }
        failure { echo 'Pipeline failed. Check the logs.' }
    }
}