pipeline {
    agent any

    stages {
        stage('Install Apache (httpd)') {
            steps {
                script {
                    sh '''
                    if ! /opt/homebrew/bin/brew list | grep -q httpd; then
                        echo "Installing Apache..."
                        /opt/homebrew/bin/brew install httpd
                    else
                        echo "Apache is already installed."
                    fi
                    '''
                }
            }
        }

        stage('Start Apache Server') {
            steps {
                script {
                    sh '''
                    echo "Starting Apache..."
                    /opt/homebrew/bin/brew services start httpd
                    '''
                }
            }
        }

        stage('Check Apache Status') {
            steps {
                script {
                    sh '''
                    echo "Checking Apache status..."
                    /opt/homebrew/bin/brew services list | grep httpd
                    '''
                }
            }
        }

        stage('Verify Apache is Running') {
            steps {
                script {
                    sh '''
                    echo "Checking if Apache responds..."
                    curl -I http://localhost:8080 || echo "Apache is not responding!"
                    '''
                }
            }
        }
    }
}
