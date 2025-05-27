 pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t my-app .'
            }
        }

        stage('Test') {
            steps {
                // Add test commands here if any
                echo 'Running tests...'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 3000:3000 my-app'
            }
        }
    }
}
