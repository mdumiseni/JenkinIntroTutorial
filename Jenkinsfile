pipeline {
    agent any

    environment {
        DOTNET_ROOT = 'C:\\Program Files\\dotnet'
        PATH = "${env.PATH};${env.DOTNET_ROOT}"
        BUILD_CONFIG = 'Release'
        DEPLOY_DIR = 'C:\\inetpub\\wwwroot\\JenkinsTutorialWebsite'
    }
    stages {

        stage('build') { 
            steps {
                echo 'building Hello World'
                bat 'dotnet --version'
            }
        }
        stage('checkout') {
            steps {
                echo 'checking out code : https://github.com/mdumiseni/JenkinIntroTutorial.git'
                checkout scm
            }
        }

        stage('Restore Dependencies') {
            steps {
                // Restores NuGet packages
                bat 'dotnet restore .\\JenkinsTutorialWebsite\\JenkinsTutorialWebsite.csproj'
            }
        }
        stage('Build Code') {
            steps {
                // Compiles the .NET application
                bat "dotnet build ./JenkinsTutorialWebsite --configuration ${BUILD_CONFIG} --no-restore"
            }
        }
        stage('Publish') {
            steps {
                // Compiles the .NET application
                bat "dotnet publish ./JenkinsTutorialWebsite/JenkinsTutorialWebsite.csproj -c Release -o ${PUBLISH_DIR} --no-build"
            }
        }

        stage('Deploy') {
            steps {
                   bat """
                echo Deploying WITHOUT IIS restart...

                robocopy %PUBLISH_DIR% %DEPLOY_DIR% /MIR /R:2 /W:2

                echo Deployment complete
                """
            }
        }
      
    }
    post {
    success {
        echo '✅ Deployment successful!'
    }
    failure {
        echo '❌ Deployment failed!'
    }
}
}
