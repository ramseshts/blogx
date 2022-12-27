pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
    }
    stages {
        stage('Build Image') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'staging') {
                sh 'docker build -t ramses01/blogx:0.0.$BUILD_NUMBER-staging .'
                    }
                    else if (env.BRANCH_NAME == 'master') {
                sh 'docker build -t ramses01/blogx:0.0.$BUILD_NUMBER-master .'
                    }
                    else
                    {
                        sh 'echo Nothing to Build'
                    }
                }
            }
        }
        stage('Push to Registry') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'staging') {
                sh 'docker push ramses01/blogx:0.0.$BUILD_NUMBER-staging'
                    }
                    else if (env.BRANCH_NAME == 'master') {
                sh 'docker push ramses01/blogx:0.0.$BUILD_NUMBER-master'
                    }
                    else
                    {
                        sh 'echo Nothing to Push'
                    }
                }
            }
        }
    }
        post {
            success {
                slackSend channel: '#jenkins',
                color: 'good',
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            }
            failure {
                slackSend channel: '#jenkins',
                color: 'danger',
                message: "*${currentBuild.currentResult}:* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}\n More info at: ${env.BUILD_URL}"
            }
        }
    }
