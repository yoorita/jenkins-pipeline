pipeline {
    agent {
        label 'alpine'
    }

    stages {
        stage('Install Apache2') {
            steps {
                sh '''
                    sudo apk update
                    sudo apk add apache2
                    sudo sh -c 'echo "ServerName localhost" >> /etc/apache2/httpd.conf'
                    sudo rc-service apache2 start

                    for i in $(seq 1 10); do
                        if ss -ltn | grep -q ":80"; then
                            echo "Apache is ready!"
                            break
                        fi
                        sleep 1
                    done
                '''
            }
        }
        stage('Check Apache2 Logs for Errors') {
            steps {
                script {
                    def errors = sh(script: '''
                        sudo sh -c 'tail -n 100 /var/log/apache2/access.log | grep -E " 4[0-9]{2} | 5[0-9]{2}" || true'
                    ''', returnStdout: true).trim()

                    if (errors) {
                        echo "Found errors in Apache logs:"
                        echo errors
                    } else {
                        echo "No 4xx or 5xx errors found."
                    }
                }
            }
        }

        stage('Stop Apache2') {
            steps {
                sh 'sudo rc-service apache2 stop'
            }
        }
    }
}
