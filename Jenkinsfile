pipeline {
    agent any

    environment {
        DOTNET_ROOT = 'C:\\Program Files\\dotnet'
        PATH = "${env.PATH};${env.DOTNET_ROOT}"
    }
    stages {
        
        stage('checkout') {
            steps {
                echo 'checking out code'
                git 'https://github.com/mdumiseni/JenkinIntroTutorial.git'
            }
        }
        stage('build') { 
            steps {
                echo 'building Hello World'
                bat 'dotnet --version'
            }
        }
        stage('Restore Dependencies') {
            steps {
                // Restores NuGet packages
                bat 'dotnet restore'
            }
        }
        stage('Build Code') {
            steps {
                // Compiles the .NET application
                bat "dotnet build --configuration ${BUILD_CONFIG} --no-restore"
            }
        }
      
    }
}
