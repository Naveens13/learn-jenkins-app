pipeline {
    agent any
    environment{
        NETLIFY_SITE_ID = "3a62ac38-b398-46d8-9839-4dce88823fe1"
        NETLIFY_AUTH_TOKEN = credentials('netlify_access_token')
        REACT_APP_VERSION = "1.0.$BUILD_ID"
    }
    stages {
         
        stage ('Docker Build') {
            steps {
                sh ''' 
                    echo "Building Docker image"
                    docker build -t my-node-image .
                '''
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'my-node-image'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    ls -la
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('AWS') {
            agent {
                docker {
                    image 'amazon/aws-cli'
                    args "--entrypoint=''"
                    reuseNode true
                }
            }
            environment{
                AWS_S3_BUCKET = "jenkinsdemobucket10929348"
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'AWS Account Jenkins', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        // aws s3 sync build s3://$AWS_S3_BUCKET
                    '''
                }
                
            }
        }

    //     stage('Test') {
    //         parallel { 
    //             stage('Unit Test') {
    //                 agent {
    //                     docker {
    //                         image 'my-node-image'
    //                         reuseNode true
    //                     }
    //                 }
                    
    //                 steps {
    //                     sh '''
    //                         #test -f build/index.html
    //                         npm test
    //                     '''
    //                 }
    //                 post {
    //                     always {
    //                         junit 'jest-results/junit.xml'
    //                     }
    //                 }
    //             }

    //             stage('E2E Test') {
    //                 agent {
    //                     docker {
    //                         image 'my-node-image'
    //                         reuseNode true
    //                     }
    //                 }
    //                 steps {
    //                     sh '''
    //                         serve -s build &
    //                         sleep 10
    //                         npx playwright test --reporter=html
    //                     '''
    //                 }   
    //                 post {
    //                     always {
    //                         publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Local HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    //                     }
    //                 }
    //             }
    //         }
    //     }

    //     stage('Netlify Staging Deploy & E2E Test') {
    //         agent {
    //             docker {
    //                 image 'my-node-image'
    //                 reuseNode true
    //             }
    //         }
    //         environment {
    //             CI_ENVIRONMENT_URL = "still not defined"
    //         }
    //         steps {
    //             sh '''
    //                 echo "Build id is: $REACT_APP_VERSION"
    //                 netlify status
    //                 netlify deploy --dir=build --json > deploy_output_staging.json
    //                 CI_ENVIRONMENT_URL=$(node-jq -r '.deploy_url' deploy_output_staging.json)
    //                 npx playwright test --reporter=html
    //             '''
    //         }   
    //         post {
    //             always {
    //                 publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Netlify Staging HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    //             }
    //         }
    //     }

    // /*
    //     stage("Manual Approval"){
    //         steps{
    //             timeout(time:1, unit:"MINUTES"){
    //                 input message: "Ready for Production Deployment" , ok: "Proceed"
    //             }
    //         }
    //     }
    // */

    //     stage('Netlify Production Deploy & Production E2E Test') {
    //         agent {
    //             docker {
    //                 image 'my-node-image'
    //                 reuseNode true
    //             }
    //         }
    //         environment {
    //             CI_ENVIRONMENT_URL = "https://splendorous-mandazi-074b97.netlify.app"
    //         }
    //         steps {
    //             sh '''
    //                 echo "Inside Netlify - Production"
    //                 netlify status
    //                 netlify deploy --prod --dir=build
    //                 echo "Inside Netlify Prod - Testing"
    //                 npx playwright test --reporter=html
    //             '''
    //         }   
    //         post {
    //             always {
    //                 publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Production HTML Report', reportTitles: '', useWrapperFileDirectly: true])
    //             }
    //         }
    //     }
    }
}