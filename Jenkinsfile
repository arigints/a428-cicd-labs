node {
    stage('Build') {
        env.PATH = "${tool 'NodeJS'}/bin:${env.PATH}"
        docker.image('node:16-buster-slim').withRun('-p 3000:3000'){
            sh 'ls'
            sh 'npm install'
        }
            
        stage('Test') { 
            sh './jenkins/scripts/test.sh' 
        }
    }
}
