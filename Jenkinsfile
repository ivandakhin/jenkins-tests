pipeline {
    agent any

    environment {
        BREW_PATH = "/opt/homebrew/bin/brew"
        APACHE_CONF = "/opt/homebrew/etc/httpd/httpd.conf"
        LOG_FILE = "/opt/homebrew/var/log/httpd/error_log"
    }

    stages {
        stage('Install Apache (httpd)') {
            steps {
                script {
                    sh '''
                    if ! ${BREW_PATH} list | grep -q httpd; then
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
                    ${BREW_PATH} services restart httpd
                    '''
                }
            }
        }

        stage('Check Apache Status') {
            steps {
                script {
                    sh '''
                    ${BREW_PATH} services list | grep httpd || exit 1
                    '''
                }
            }
        }
    }
}
