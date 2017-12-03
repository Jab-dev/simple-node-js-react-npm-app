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
                echo "Listing folders:"
                ls ./
            }
        }
        stage('Deploy') {
            if (env.BRANCH_NAME == 'master') {
                steps {
                    echo 'Deploying master branch'
                    sh 'npm run build'   
                }
            } else {
                echo 'There is no need to deploy'
            }
        }
    }
}
