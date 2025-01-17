pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                echo 'Hello World'
                sh ''' 
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('test'){
            steps{
                sh ''' 
                    echo "Test stage ..."
                    test -f build/index.html
                    echo "${?}"
                    npm test
                '''
            }
        }
    }
}