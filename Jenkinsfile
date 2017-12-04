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
        stage('Listing') {
            steps {
                echo 'Listing folders:'
                sh 'pwd'
                sh 'ls -la .'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Testing s3 deploy'
                withAWS(region: '', credentials: 'awss3deploy') {
                    s3Upload(
                        file: 'Jenkinsfile',
                        bucket: 'jenkins-pipeline-integration-test',
                        path: 'jenkins/'
                    )
                }
            }
        }
    }
}