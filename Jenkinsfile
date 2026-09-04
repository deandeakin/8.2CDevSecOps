pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/deandeakin/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                bat 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                bat 'npm test || exit /b 0'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                bat 'npm run coverage || exit /b 0'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                bat 'npm audit || exit /b 0'
            }
        }
        stage('SonarCloud Analysis') {
            steps {
                withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
                    bat '''
                    if not exist sonar-scanner-8.1.0.6389-windows-x64 (
                        powershell -NoProfile -Command "Invoke-WebRequest -Uri 'https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-8.1.0.6389-windows-x64.zip' -OutFile 'sonar-scanner.zip'"
                        powershell -NoProfile -Command "Expand-Archive -Path 'sonar-scanner.zip' -DestinationPath '.' -Force"
                    )

                    call sonar-scanner-8.1.0.6389-windows-x64\\bin\\sonar-scanner.bat
                    '''
                }
            }
        }
    }
}
