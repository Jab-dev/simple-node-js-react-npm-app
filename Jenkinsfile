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
            when {
                branch 'refs/heads/master'
            }
            steps {
                echo 'Deploying master branch'
                sh 'npm run build'   
            }
        }
    }
}