pipeline {
    agent any

    environment {
        CHROME_VERSION = 'google-chrome-stable=127.0.6533.73-1'
        CHROMEDRIVER_VERSION = '127.0.6533.72'
        CHROMEDRIVER_INSTALL_PATH = '/usr/local/bin/chromedriver'
    }

    stages {
        stage('Checkout code') {
            steps {
                git branch: 'main', url: 'https://github.com/Dreamheart92/Selenium-IDE.git'
            }
        }

        stage('Set up .NET Core') {
            steps {
                sh '''
                echo Installing .NET SDK 6.0
                wget https://packages.microsoft.com/config/ubuntu/22.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb
                sudo dpkg -i packages-microsoft-prod.deb
                sudo apt-get update
                sudo apt-get install -y apt-transport-https
                sudo apt-get update
                sudo apt-get install -y dotnet-sdk-6.0
                '''
            }
        }

        stage('Uninstall Current Chrome') {
            steps {
                sh '''
                echo Uninstalling current Chrome
                sudo apt-get remove -y google-chrome-stable || true
                '''
            }
        }

        stage('Install Specific Chrome Version') {
            steps {
                sh '''
                echo Installing Chrome version $CHROME_VERSION
                wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
                sudo apt-get install -y ./google-chrome-stable_current_amd64.deb
                google-chrome --version
                '''
            }
        }

        stage('Download and Install ChromeDriver') {
            steps {
                sh '''
                echo Downloading ChromeDriver version $CHROMEDRIVER_VERSION
                wget https://chromedriver.storage.googleapis.com/$CHROMEDRIVER_VERSION/chromedriver_linux64.zip
                unzip chromedriver_linux64.zip
                sudo mv chromedriver $CHROMEDRIVER_INSTALL_PATH
                sudo chmod +x $CHROMEDRIVER_INSTALL_PATH
                $CHROMEDRIVER_INSTALL_PATH --version
                '''
            }
        }

        stage('Restore Dependencies') {
            steps {
                sh 'dotnet restore SeleniumIde.sln'
            }
        }

        stage('Build') {
            steps {
                sh 'dotnet build SeleniumIde.sln --configuration Release'
            }
        }
    }
}
