pipeline {
    agent any

    stages {
        stage('Run docker') {
            agent {
                docker {
                    image 'node:22-alpine'
                }
            }
            steps {
                echo 'Hello from Github'
                sh '''
                    echo "NPM Version: npm --version"
                '''
            }
        }
    }
}
