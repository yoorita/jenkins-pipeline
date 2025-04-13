pipeline {
    agent {
        label 'alpine'
    }

    stages {
        stage('Update') {
            steps {
                echo 'Update packages'
                sh 'sudo apk update'
            }
        }
    }
}
