pipeline {
    agent any

    environment{
        NETLIFY_SITE_ID = '2ac404e5-ef4a-4b07-b037-9c6d308f56db'
        NETLIFY_AUTH_TOKEN = credentials('netlify_access_token')
    }
    stages {

        // stage('Build') {
        //     agent{
        //         docker{
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps {
        //         echo 'Hello World'
        //         sh ''' 
        //             npm --version
        //             npm ci
        //             npm run build
        //             ls -la
        //         '''
        //     }
        // }
        // stage('test'){
        //     agent{
        //         docker{
        //             image 'node:18-alpine'
        //             reuseNode true
        //         }
        //     }
        //     steps{
        //         sh ''' 
        //             echo "Test stage ..."
        //             test -f build/index.html
        //             echo "${?}"
        //             npm test
        //         '''
        //     }
        // }

        stage ('deploy'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                '''
            }
        }
    }
    // post{
    //     always{
    //         junit 'test-results/junit.xml'
    //     }
    // }
}
