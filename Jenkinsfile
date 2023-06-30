node {
    stage('Build') {
        withDockerContainer(image: 'node:16-buster-slim', args: '-p 3000:3000') {
            sh 'npm install'
        }
    stage('Test') { 
        withDockerContainer(image: 'node:16-buster-slim', args: '-p 3000:3000') {
            sh './jenkins/scripts/test.sh' 
            }
        }
    }
}
