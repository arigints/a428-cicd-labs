node {
    stage('Build') {
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            checkout scm
            sh 'npm install'
        }
    }
    stage('Test') {
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            sh './jenkins/scripts/test.sh' 
            input message: 'Lanjutkan ke tahap deploy? (Click "Proceed" to continue)'
        }
    }
    stage('Deploy') { 
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            sh './jenkins/scripts/deliver.sh'
            sleep(60)
            sh './jenkins/scripts/kill.sh'
        }
        sshagent(['production']) {
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 pwd"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 sudo rm -r /home/cloud/a428-cicd-labs"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 git clone -b react-app https://github.com/arigints/a428-cicd-labs.git"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 sudo docker build -t newreactimg /home/cloud/a428-cicd-labs"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 docker rmi react-app:latest -f"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 docker tag newreactimg:latest react-app:latest"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 docker rmi newreactimg:latest"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 docker rm -f react-app"
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12 docker run --name react-app -d -p 3001:3000 react-app"
        }
    }
}