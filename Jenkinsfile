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
                bat "dotnet publish ./JenkinsTutorialWebsite/JenkinsTutorialWebsite.csproj -c Release -o publish"
            }
        }

        stage('Deploy') {
            steps {
                  bat """
                    echo Stopping App Pool...

                    %windir%\\system32\\inetsrv\\appcmd stop apppool /apppool.name:"JenkinsTutorialWebsiteAppPool"

                    echo Deploying files...

                    robocopy publish C:\\inetpub\\wwwroot\\JenkinsTutorialWebsite /MIR /R:1 /W:1

                    echo Starting App Pool...

                    %windir%\\system32\\inetsrv\\appcmd start apppool /apppool.name:"JenkinsTutorialWebsiteAppPool"
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
