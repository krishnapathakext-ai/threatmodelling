pipeline {
    agent any

    environment {
        SEEZO_API_TOKEN = credentials('OyWhrWtWpEFhSVPFtWtWnDbd-3c2TIM6WpcpWjjN2Jc')

        SEEZO_BASE_URL = 'https://app.seezo.io'
        PROJECT_ID     = '65bd495c-a694-4443-a97e-067b8d4ccac7'
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

        stage('Upload Architecture to Seezo') {
            steps {

                bat '''
                curl -X POST "%SEEZO_BASE_URL%/api/v1/projects/%PROJECT_ID%/assessments/" ^
                -H "Authorization: Bearer %SEEZO_API_TOKEN%" ^
                -H "Accept: application/json" ^
                -F "feature_name=threatmodelling" ^
                -F "resources=@threatmodel.png"
                '''
            }
        }

        stage('Completed') {
            steps {
                echo 'Seezo Integration Completed Successfully'
            }
        }
    }
}
