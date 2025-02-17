pipeline {
    agent any

    stages {
        stage('Install Apache (httpd)') {
            steps {
                script {
                    sh '''
                    if ! /opt/homebrew/bin/brew list | grep -q httpd; then
                        echo "Installing Apache..."
                        brew install httpd
                    else
                        echo "Apache is already installed."
                    fi
                    '''
                }
            }
        }

        stage('Find Available Port') {
            steps {
                script {
                    sh '''
                    APACHE_PORT=8080
                    if lsof -i :8080 | grep LISTEN; then
                        echo "Port 8080 is in use. Switching Apache to port 3000."
                        APACHE_PORT=3000
                    fi
                    echo "Using port $APACHE_PORT for Apache"

                    # Меняем порт в конфиге Apache
                    sudo sed -i '' "s/^Listen [0-9]*/Listen $APACHE_PORT/" /opt/homebrew/etc/httpd/httpd.conf
                    '''
                }
            }
        }

        stage('Restart Apache') {
            steps {
                script {
                    sh '''
                    echo "Restarting Apache..."
                    brew services restart httpd
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
                    APACHE_PORT=$(grep -oE '^Listen [0-9]+' /opt/homebrew/etc/httpd/httpd.conf | awk '{print $2}')
                    echo "Checking if Apache responds on port $APACHE_PORT..."
                    curl -I http://localhost:$APACHE_PORT || echo "Apache is not responding!"
                    '''
                }
            }
        }
    }
}
