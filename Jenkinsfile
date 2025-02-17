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

        stage('Change Apache Port to 3000') {
            steps {
                script {
                    sh '''
                    echo "Changing Apache port to 3000..."
                    sudo sed -i '' 's/^Listen 8080/Listen 3000/' /opt/homebrew/etc/httpd/httpd.conf
                    '''
                }
            }
        }

        stage('Start Apache Server') {
            steps {
                script {
                    sh '''
                    echo "Starting Apache..."
                    sudo brew services restart httpd
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

        stage('Verify Apache is Running') {
            steps {
                script {
                    sh '''
                    echo "Checking if Apache responds..."
                    curl -I http://localhost:3000 || echo "Apache is not responding!"
                    '''
                }
            }
        }
    }
}
