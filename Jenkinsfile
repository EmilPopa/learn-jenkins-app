pipeline {
    agent any

    stages {
        stage('Install & Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Installing dependencies and building project...'
                sh '''
                    node --version
                    npm --version
                    npm ci
                    npm run build
                '''
            }
        }

        stage('Run Tests') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Running tests with Jest...'
                // Creăm folderul unde va fi raportul JUnit
                sh 'mkdir -p test-results'
                // Rulează testele în modul CI și generează raport JUnit
                sh 'CI=true npx react-scripts test --reporters=default --reporters=jest-junit'
                // Verificăm că raportul există
                sh 'ls -la test-results'
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.54.0-noble'
                    reuseNode true
                    }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build & 
                    sleep 10
                    npx playright test
                '''
            }
        }
    }

    post {
        always {
            // Publicăm raportul JUnit în Jenkins indiferent de rezultat
            junit 'test-results/junit.xml'
        }
    }
}
