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
                echo "Deploying branch ${env.BRANCH_NAME}"
                sh 'npm run build'
                withAWS(region: 'eu-west-1', credentials: 'awss3deploy') {
                    s3Upload(
                        file: 'build',
                        bucket: 'jenkins-pipeline-integration-test',
                        path: "${env.BRANCH_NAME}"
                    )
                }
            }
        }
    }
}