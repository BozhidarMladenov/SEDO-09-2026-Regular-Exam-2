pipeline {
    agent any

    triggers {
        // Poll every minute — replace with a GitHub webhook in production
        pollSCM('* * * * *')
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Restore') {
            steps {
                sh 'dotnet restore'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build --no-restore --configuration Release'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'dotnet test Homies.Tests/Homies.Tests.csproj \
                  --no-build --configuration Release --verbosity normal'
            }
        }

        stage('Integration Tests') {
            steps {
                sh 'dotnet test Homies.IntegrationTests/Homies.IntegrationTests.csproj \
                  --no-build --configuration Release --verbosity normal'
            }
        }
    }

    post {
        success { echo '✅ All stages passed!' }
        failure { echo '❌ Build or tests failed!' }
    }
}