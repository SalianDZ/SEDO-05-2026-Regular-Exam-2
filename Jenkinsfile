pipeline {
    agent any

    options {
        timestamps()
        skipDefaultCheckout(false)
    }

    environment {
        DOTNET_CLI_TELEMETRY_OPTOUT = '1'
        DOTNET_NOLOGO = 'true'
        SOLUTION = 'Homies.sln'
    }

    stages {
        stage('Restore') {
            when { branch 'main' }
            steps {
                sh 'dotnet restore "$SOLUTION"'
            }
        }

        stage('Build') {
            when { branch 'main' }
            steps {
                sh 'dotnet build "$SOLUTION" --configuration Release --no-restore'
            }
        }

        stage('Test') {
            when { branch 'main' }
            steps {
                sh 'dotnet test "$SOLUTION" --configuration Release --no-build --verbosity normal --logger "trx;LogFileName=test-results.trx"'
            }
            post {
                always {
                    archiveArtifacts artifacts: '**/TestResults/*.trx', allowEmptyArchive: true, fingerprint: true
                }
            }
        }
    }

    post {
        success {
            echo 'Build and tests completed successfully.'
        }
        failure {
            echo 'Build or tests failed.'
        }
    }
}
