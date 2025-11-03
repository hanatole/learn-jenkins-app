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

        stage("Test"){
            agent {
                docker{
                    image "node:22-alpine"
                    reuseNode true
                }
            }

            steps{
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }
}
