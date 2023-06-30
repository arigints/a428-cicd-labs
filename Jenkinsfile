node {
    stage('Build') {
        withDockerContainer(image: 'node:16-buster-slim', args: '-p 3000:3000') {
            sh 'npm install'
        }
    }
}
//withDockerContainer(image: 'node:16-buster-slim', args: “-p '-p 3000:3000')
//docker.image('node:16-buster-slim').inside('-p 3000:3000') 