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