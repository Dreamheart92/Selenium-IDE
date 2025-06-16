pipeline {
    agent any

    environment {
        CHROME_VERSION = '127.0.6533.73'
        CHROMEDRIVER_VERSION = '127.0.6533.72'
        CHROME_INSTALL_PATH = 'C:\\Program Files\\Google\\Chrome\\Application'
        CHROMEDRIVER_INSTALL_PATH = 'C:\\Program Files\\Google\\Chrome\\Application\\chromedriver.exe'
    }

    stages {
        stage('Checkout code') {
            steps {
                git branch 'main', url: 'https://github.com/Dreamheart92/Selenium-IDE.git'
            }
        }

        stage('Set up .NET Core') {
            steps {
                bat '''
                echo Installing .NET SDK 6.0
                choco install dotnet-sdk --version 6.0.100 --yes
                '''
            }
        }

        stage('Uninstall Current Chrome') {
            steps {
                bat '''
                echo Uninstalling current Chrome
                choco uninstall googlechrome --yes
                '''
            }
        }

        stage('Install Specific Chrome Version') {
            steps {
                bat '''
                echo Installing Chrome version %CHROME_VERSION%
                choco install googlechrome --version %CHROME_VERSION% --yes --alow-downgrade --ignore-checksums
                '''
            }
        }

        stage('Download and Install ChromeDriver') {
            steps {
                bat '''
                 echo Downloading ChromeDriver version %CHROMEDRIVER_VERSION%
                 powershell -command "Invoke-WebRequest -Uri https://chromedriver.storage.googleapis.com/%CHROMEDRIVER_VERSION%/chromedriver_win32.zip -OutFile chromedriver.zip -UseBasicParsing"
                  powershell -command "Expand-Archive -Path chromedriver.zip -DestinationPath ."
                  powershell -command "Move-Item -Path .\\chromedriver.exe -Destination '%CHROME_INSTALL_PATH%\\chromedriver.exe' -Force"
                  '''
            }
        }

        stage('Restore Dependencies') {
            steps {
                bat 'dotnet restore SeleiniumIde.sln'
            }
        }

        stage('Build') {
            steps {
                bat 'dotnet build SeleiniumIde.sln --configuration Release'
            }
        }
    }
}
