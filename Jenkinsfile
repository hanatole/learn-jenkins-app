pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = '9a23559-eb4b-47c2-8ef1-7e175b1d246f'
        NETLIFY_AUTH_TOKEN = credentials("netlify-token")
    }

    stages {
        stage('Build') {
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

            post{
                always{
                    junit 'jest-results/junit.xml'
                }
            }
        }

        stage("Test"){
            parallel{
                stage("Unittest"){
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

                stage("E2E"){
                    agent {
                        docker{
                            image "mcr.microsoft.com/playwright:v1.39.0-jammy"
                            reuseNode true
                        }
                    }

                    steps{
                        sh '''
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                        '''
                    }
                }
            }
        }

        stage("Deploy"){
            agent{
                docker{
                    image "node:22-alpine"
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install netlify-cli
                    echo "Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify deploy --dir build --prod

                '''
            }
        }
    }
}
