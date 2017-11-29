pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                bash 'npm install' 
            }
        }
	stage('Test') { 
            steps {
                bash 'npm run test' 
            }
        }

    }
}
