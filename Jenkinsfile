pipeline {
    agent any

    environment {
        BREW_PATH = "/opt/homebrew/bin/brew"  // Для Apple Silicon (M1/M2/M3) | На Intel Mac → /usr/local/bin/brew
        APACHE_CONF = "/opt/homebrew/etc/httpd/httpd.conf"
        LOG_FILE = "/opt/homebrew/var/log/httpd/error_log"
    }

    stages {
        stage('Install Apache (httpd)') {
            steps {
                script {
                    sh '''
                    if ! ${BREW_PATH} list | grep -q httpd; then
                        echo "Installing Apache..."
                        ${BREW_PATH} install httpd
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
                    ${BREW_PATH} services restart httpd
                    '''
                }
            }
        }

        stage('Check Apache Status') {
            steps {
                script {
                    sh '''
                    echo "Checking Apache status..."
                    ${BREW_PATH} services list | grep httpd || exit 1
                    '''
                }
            }
        }

        stage('Check Apache Logs') {
            steps {
                script {
                    sh '''
                    if [ -f "${LOG_FILE}" ]; then
                        echo "Checking for 4xx and 5xx errors in logs..."
                        grep -E ' (4[0-9]{2}|5[0-9]{2}) ' "${LOG_FILE}" || echo "No errors found."
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
