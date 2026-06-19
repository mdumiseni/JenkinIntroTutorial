pipeline {
    agent any

    environment {
        DOTNET_ROOT = 'C:\\Program Files\\dotnet'
        PATH = "${env.PATH};${env.DOTNET_ROOT}"
        BUILD_CONFIG = 'Release'
        PUBLISH_DIR = "${env.WORKSPACE}\\publish"
        IIS_PATH = 'C:\\inetpub\\wwwroot\\JenkinsTutorialWebsite'
        APP_POOL = 'JenkinsTutorialWebsiteAppPool'
        IIS_SITE = 'JenkinsTutorialWebsite'
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
                
                bat "dotnet publish ./JenkinsTutorialWebsite --configuration Release --output \"%PUBLISH_DIR%\""
            }
        }

        stage('Deploy to IIS') {
            steps {
                 powershell '''
                        $ErrorActionPreference = "Stop"
                        Import-Module WebAdministration
                $appPool = "$env:APP_POOL"
                        $siteName = "$env:IIS_SITE"
                        $iisPath = "$env:IIS_PATH"
                        $publishDir = "$env:PUBLISH_DIR"
                # Create App Pool if it doesn't exist
                        if (-not (Test-Path "IIS:\\AppPools\\$appPool")) {
                            Write-Host "Creating App Pool: $appPool"
                            New-WebAppPool -Name $appPool
                        } else {
                            Write-Host "App Pool $appPool already exists"
                        }
                # Create physical path if not exists
                        if (-not (Test-Path $iisPath)) {
                            Write-Host "Creating directory: $iisPath"
                            New-Item -Path $iisPath -ItemType Directory | Out-Null
                        }
                # Create site if it doesn't exist
                        if (-not (Get-Website -Name $siteName -ErrorAction SilentlyContinue)) {
                            Write-Host "Creating IIS site: $siteName"
                            New-Website -Name $siteName -Port 8085 -PhysicalPath $iisPath -ApplicationPool $appPool
                        } else {
                            Write-Host "IIS site $siteName already exists"
                        }
                # Stop site before deploying
                        if ((Get-Website -Name $siteName).State -eq "Started") {
                            Stop-Website -Name $siteName
                        }
                # Deploy files
                        Write-Host "Copying files from $publishDir to $iisPath"
                        Copy-Item -Path "$publishDir\\*" -Destination $iisPath -Recurse -Force
                # Start site after deployment
                        Start-Website -Name $siteName
                Write-Host "Deployment completed successfully."
                        '''
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
