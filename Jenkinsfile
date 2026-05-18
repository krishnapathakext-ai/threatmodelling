pipeline {
    agent any

    environment {

        SEEZO_BASE_URL = 'https://app.seezo.io'
        PROJECT_ID     = '2a473597-85c8-41ae-945f-74d9c85e9188'

        SEEZO_API_TOKEN = 'OyWhrWtWpEFhSVPFtWtWnDbd-3c2TIM6WpcpWjjN2Jc'
    }

   stages {

        stage('Checkout') {
            steps {

                git branch: 'main',
                url: 'https://github.com/Krishna-Gopal-Pathak/threatmodelling.git'
            }
        }

        stage('Verify Files') {
            steps {

                bat 'dir'
            }
        }

        stage('Upload PNG to Seezo') {
            steps {

                bat '''
                curl -X POST "%SEEZO_BASE_URL%/api/v1/projects/%PROJECT_ID%/assessments/" ^
                -H "Authorization: Bearer %SEEZO_API_TOKEN%" ^
                -H "Accept: application/json" ^
                -F "file_0=@HLD_DFD.png"
                '''
            }
        }

        stage('Completed') {
            steps {

                echo 'Seezo Integration Completed Successfully'
            }
        }
    }

    post {

        success {

            echo 'Pipeline executed successfully.'
        }

        failure {

            echo 'Pipeline failed.'
        }
    }
}
