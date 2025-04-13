pipeline {
    agent {
        label 'alpine'
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello World'
                sh 'sudo -l'
            }
        }
    }
}
