pipeline {
    agent any

    stages {
        stage('Checkout code') {
            // Checkout the repository
            steps {
                git branch: 'main', url: 'https://github.com/RIG0708/JenkinsSeleniumIdeDemoRepo_12.08'
            }
        }

        stage('Set up .Net Core') {
            // Install .NET SDK
            steps {
                bat '''
                echo Downloading .Net 6 SDK
                curl -l -o dotnet-sdk-6.0.132-win-x86.exe https://download.visualstudio.microsoft.com/download/pr/ad59f1d1-5f19-4474-86be-2f09ab195618/5c7a64445dae84e386bb88e1f6ac09e4/dotnet-sdk-6.0.132-win-x86.exe 
                echo Installing dotnet-sdk-6.0.132-win-x86.exe
                dotnet-sdk-6.0.132-win-x86.exe /quiet /norestart
                dotnet --version
                '''
            }
        }

        stage('Restoring nuget Packages') {
            // Restore NuGet packages
            steps {
                bat 'dotnet restore SeleniumIde.sln'
            }
        }

        stage('Build') {
            // Build the project
            steps {
                bat 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }

        stage('Run Test') {
            // Run tests
            steps {
                bat 'dotnet test SeleniumIde.sln --logger "trx;LogFileName=TestResults.trx"'
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true
            step([
                $class: 'MSTestPublisher',
                testResultsFile: '**/TestResults/*.trx'
            ])
        }
    }
}
