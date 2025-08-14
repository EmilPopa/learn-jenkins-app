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

                // Instalăm jest-junit (dacă nu e deja instalat în package.json)
                sh '''
                    npm install --save-dev jest-junit
                '''

                // Rulăm testele în mod CI și generăm raportul JUnit
                sh '''
                    test -f build/index.html
                    CI=true npx jest --reporters=default --reporters=jest-junit
                '''

                // Publicăm raportul în Jenkins
                junit 'junit.xml'
            }
        }
    }
}
