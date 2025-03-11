pipeline {
    agent any

    stages {
        /*
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
                ls -la
                '''
            }
        }
        */
    stage ('Test') {
        agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
        steps {
            sh '''
            #test -f build/index.html
            npm test
        '''
    }
}
stage ('E2E') {
        agent {
                docker {
withDockerContainer(image: 'mcr.microsoft.com/playwright:v1.51.0-noble', args: '--user root') {
    sh 'npx playwright install --with-deps'
    }
                    reuseNode true
                }
            }
        steps {
            sh '''
            npm install serve
            node_modules/.bin/serve -s build &
            sleep 10
            npx playwright test
        '''
    }
}
}
post {
    always {
        junit 'test-results/junit.xml'
    }
}
}

