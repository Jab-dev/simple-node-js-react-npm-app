pipeline {
    agent {
        docker {
            image 'node:8-alpine' 
            args '-p 3000:3000' 
        }
    }
    environment {
	   CI = 'true'
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    	stage('Test') { 
            steps {
                sh './jenkins/scripts/test.sh' 
            }
        }
        stage('Deploy') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'master') {
                        echo 'There is master branch, deploying result'
                        echo 'Building proyect'
                        npm run build
                        echo 'Testing s3 deploy'
                        withAWS(region: 'eu-west-1', credentials: 'awss3deploy') {
                            s3Upload(
                                file: 'build',
                                bucket: 'jenkins-pipeline-integration-test',
                                path: ''
                            )
                        }
                    } else {
                        echo 'Is not master branch, no need to deploy'
                    }
                }
            }
        }
    }
}