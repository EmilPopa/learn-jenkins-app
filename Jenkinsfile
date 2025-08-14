pipeline {
    agent any

    stages {
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la build
                '''
            }
        }

        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                     reuseNode true
                }
            }
            steps {
                echo 'Running tests...'

                sh '''
                    mkdir -p test-results
                    CI=true npx react-scripts test --reporters=default --reporters=jest-junit
                    ls -la test-results
                '''

                // citește raportul JUnit din workspace
                junit 'test-results/junit.xml'
            }
        }

    }
}
