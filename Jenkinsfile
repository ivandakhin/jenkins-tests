pipeline {
    agent any

    stages {
        stage('Install Apache (httpd)') {
            steps {
                script {
                    sh '''
                    if ! brew list | grep -q httpd; then
                        echo "Installing Apache..."
                        brew install httpd
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
                    brew services start httpd
                    '''
                }
            }
        }

        stage('Check Apache Status') {
            steps {
                script {
                    sh '''
                    echo "Checking Apache status..."
                    brew services list | grep httpd
                    '''
                }
            }
        }

        stage('Check Apache Logs') {
            steps {
                script {
                    sh '''
                    LOG_FILE="/opt/homebrew/var/log/httpd/error_log"
                    if [ -f "$LOG_FILE" ]; then
                        echo "Checking for 4xx and 5xx errors in logs..."
                        grep -E ' (4[0-9]{2}|5[0-9]{2}) ' "$LOG_FILE" || echo "No errors found."
                    else
                        echo "Log file not found!"
                        exit 1
                    fi
                    '''
                }
            }
        }
    }
}
