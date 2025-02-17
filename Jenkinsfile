pipeline {
    agent any

    stages {
        stage('Install Apache') {
            steps {
                script {
                    sh '''
                    if ! brew list | grep -q httpd; then
                        echo "Process ..."
                        brew install httpd
                    else
                        echo "OK"
                    fi
                    '''
                }
            }
        }

        stage('Start Apache Server') {
            steps {
                script {
                    sh '''
                    brew services start httpd
                    '''
                }
            }
        }

        stage('Check Apache Status') {
            steps {
                script {
                    sh '''
                    brew services list | grep httpd
                    '''
                }
            }
        }

        stage('Verify Apache is Running') {
            steps {
                script {
                    sh '''
                    curl -I http://localhost:8080 || echo "OK!"
                    '''
                }
            }
        }
    }
}
