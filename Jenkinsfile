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
                    sudo rc-service apache2 start
                '''
            }
        }
        stage('Check Apache Logs for Errors') {
            steps {
                script {
                    def errors = sh(script: '''
                        sudo tail -n 100 /var/log/apache2/error_log | grep -E " 4[0-9]{2} | 5[0-9]{2}"
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
    }

    post {
        always {
            echo "Stopping Apache2..."
            sh 'sudo rc-service apache2 stop'
        }
    }
}
