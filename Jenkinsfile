pipeline {
    agent any
    environment {
	   CI = 'true'
    }
    stages {
        stage('Initialize') {
            steps {
                script {
                    echo 'Initializing...'
                    def node = tool name: 'Node-8.9.2', type: 'jenkins.plugins.nodejs.tools.NodeJSInstallation'
                    env.PATH = "${node}/bin:${env.PATH}"
                }
            }
        }
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
                        sh 'npm run build'
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