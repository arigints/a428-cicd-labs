pipeline {
    agent {
        docker {
            image 'node:16-buster-slim' 
            args '-p 3000:3000' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
    }
}

/* node {
    stage('Build') {
        // Run the build steps
        docker.image('node:16-buster-slim').withRun('-p 3000:3000') { container ->
            // Run commands inside the Docker container
            sh 'npm install'
        }
    }
}
 */