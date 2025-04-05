pipeline {
    agent any
    environment {
        AZURE_CREDENTIALS_ID = 'azure-service-principal'
        RESOURCE_GROUP = 'tushar-python-resource'
        APP_SERVICE_NAME = 'python-ts-webapp'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: 'https://github.com/Tusharsankhla18/PythonWebAPIJenkins.git'
            }
        }

        stage('Build') {
    steps {
        bat 'C:\\Users\\Hp\\AppData\\Local\\Programs\\Python\\Python313\\python.exe -m venv venv'
        bat '.\\venv\\Scripts\\activate && C:\\Users\\Hp\\AppData\\Local\\Programs\\Python\\Python313\\python.exe -m pip install --upgrade pip'
        bat '.\\venv\\Scripts\\activate && C:\\Users\\Hp\\AppData\\Local\\Programs\\Python\\Python313\\python.exe -m pip install -r requirements.txt'
    }
}


        stage('Package') {
            steps {
                bat 'powershell Compress-Archive -Path * -DestinationPath PythonWebAPIJenkins.zip -Force'
            }
        }

        stage('Deploy') {
            steps {
                withCredentials([azureServicePrincipal(credentialsId: AZURE_CREDENTIALS_ID)]) {
                    bat '''
                        az login --service-principal -u %AZURE_CLIENT_ID% -p %AZURE_CLIENT_SECRET% --tenant %AZURE_TENANT_ID%
                        az webapp deploy --resource-group %RESOURCE_GROUP% --name %APP_SERVICE_NAME% --src-path app.zip --type zip
                    '''
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Deployment Failed!'
        }
    }
}
