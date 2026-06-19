pipeline {
    agent any

    environment {
        DOTNET_ROOT = 'C:\\Program Files\\dotnet'
        PATH = "${env.PATH};${env.DOTNET_ROOT}"
        BUILD_CONFIG = 'Release'
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
        stage('Release') {
            steps {
                // Compiles the .NET application
                bat "dotnet run -c Release --project ./JenkinsTutorialWebsite/JenkinsTutorialWebsite.csproj"
            }
        }
      
    }
}
