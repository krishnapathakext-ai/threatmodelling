pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Krishna-Gopal-Pathak/threatmodelling.git'
            }
        }

        stage('Hello') {
            steps {
                bat 'echo Jenkins Connected Successfully'
            }
        }
    }
}
