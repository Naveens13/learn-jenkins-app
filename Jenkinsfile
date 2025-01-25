    pipeline {
        agent any
        environment{
            NETLIFY_SITE_ID = "3a62ac38-b398-46d8-9839-4dce88823fe1"
            NETLIFY_AUTH_TOKEN = credentials('netlify_access_token')
        }
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
                        ls -la
                    '''
                }
            }

            stage('Test'){
                parallel{
                    stage('Unit Test') {
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
                    stage('E2E Test') {
                        agent {
                            docker {
                                image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                                reuseNode true
                            }
                        }

                        steps {
                            sh '''
                                npm install serve
                                node_modules/.bin/serve -s build &
                                sleep 10
                                npx playwright test --reporter=html
                            '''
                            post {
                                always {
                                    junit 'jest-results/junit.xml'
                                    publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
                                }
                            }
                        }
                    }
                }
            }

            stage('Netlify_deploy') {
                agent {
                    docker {
                        image 'node:18-alpine'
                        reuseNode true
                    }
                }
                steps {
                    sh '''
                        echo "Inside netlify build......$NETLIFY_SITE_ID -- $NETLIFY_AUTH_TOKEN"
                        npm install netlify-cli
                        node_modules/.bin/netlify --version
                        node_modules/.bin/netlify status
                    '''
                }
            }
        }

    }
