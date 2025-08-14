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

                // Rulăm cu react-scripts test, dar în mod CI și cu reporter-ul JUnit
                sh '''
                    CI=true npx react-scripts test --reporters=default --reporters=jest-junit
                '''

                junit 'junit.xml'
            }
        }
    }
}
