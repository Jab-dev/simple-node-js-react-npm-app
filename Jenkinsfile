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
                script {
                    echo 'Probando valor'
                    echo env.BRANCH_NAME
                    if (env.BRANCH_NAME == 'master') {
                        echo 'I only execute on the master branch'
                    } else {
                        echo 'I execute elsewhere'
                    }
                }
            }
        }
    }
}