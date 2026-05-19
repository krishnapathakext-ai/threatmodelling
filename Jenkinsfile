pipeline {
    agent any

    environment {

        SEEZO_BASE_URL = 'https://app.seezo.io'
        PROJECT_ID     = '2a473597-85c8-41ae-945f-74d9c85e9188'
        SEEZO_API_TOKEN = '1C9eRjwzyvYoppXrOfibncss-C3jx6Q76TYQtRsqXiY'
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

        stage('Create Assessment') {
            steps {

                script {

                    bat '''
                    curl --fail -X POST "%SEEZO_BASE_URL%/api/v1/projects/%PROJECT_ID%/assessments/" ^
                    -H "Authorization: Bearer %SEEZO_API_TOKEN%" ^
                    -H "Accept: application/json" ^
                    -F "feature_name=ThreatModelAssessment" ^
                    -F "resources_data=[{\\"type\\":\\"file_upload\\",\\"file_id\\":\\"design-doc\\",\\"classification\\":\\"primary\\"}]" ^
                    -F "design-doc=@HLD_DFD.png" ^
                    -o response.json
                    '''

                    def response = readFile('response.json')

                    echo "Create Assessment Response:"
                    echo response

                    def matcher = response =~ /"assessment_id":"([^"]+)"/

                    if (matcher) {

                        env.ASSESSMENT_ID = matcher[0][1]

                        echo "Assessment ID: ${env.ASSESSMENT_ID}"

                    } else {

                        error("Failed to extract assessment_id from response")
                    }
                }
            }
        }

        stage('Wait Before Polling') {
            steps {

                sleep time: 20, unit: 'SECONDS'
            }
        }

        stage('Check Assessment Progress') {
            steps {

                bat """
                curl --fail -X GET "%SEEZO_BASE_URL%/api/v1/projects/%PROJECT_ID%/assessments/%ASSESSMENT_ID%/progress/" ^
                -H "Authorization: Bearer %SEEZO_API_TOKEN%" ^
                -H "Accept: application/json"
                """
            }
        }

        stage('Get Assessment Details') {
            steps {

                bat """
                curl --fail -X GET "%SEEZO_BASE_URL%/api/v1/projects/%PROJECT_ID%/assessments/%ASSESSMENT_ID%/" ^
                -H "Authorization: Bearer %SEEZO_API_TOKEN%" ^
                -H "Accept: application/json"
                """
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
