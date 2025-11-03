pipeline {
    agent any

    stages {
        stage('Run docker') {
            agent {
                docker {
                    image 'node:22-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
    }
}
