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

        stage('Upload Architecture PDF to Seezo') {
            steps {

                bat '''
                curl --fail -v -X POST "%SEEZO_BASE_URL%/api/v1/projects/%PROJECT_ID%/assessments/" ^
                -H "Authorization: Bearer %SEEZO_API_TOKEN%" ^
                -H "Accept: application/json" ^
                -F "feature_name=ThreatModelAssessment" ^
                -F "resources_data=[{\\"type\\":\\"file_upload\\",\\"file_id\\":\\"design-doc\\",\\"classification\\":\\"primary\\"}]" ^
                -F "design-doc=@HLD_DFD.pdf"
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
