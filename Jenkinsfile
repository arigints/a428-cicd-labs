node {
    stage('Build') {
        sshagent(['production']) {
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12"
            sh "pwd"
            sh "rm -r /home/cloud/a428-cicd-labs"
            sh "git clone -b react-app https://github.com/arigints/a428-cicd-labs.git"
            sh "cd /home/cloud/a428-cicd-labs && docker build -t newreactimg ."
            sh "docker rm -f react-app && docker rmi react-app:latest -f && docker tag newreactimg:latest react-app:latest && docker rmi newreactimg:latest"
            sh "docker run --name react-app -d -p 3001:3000 react-app"
        }
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            checkout scm
            sh 'npm install'
        }
    }
    stage('Test') {
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            sh './jenkins/scripts/test.sh' 
            //input message: 'Lanjutkan ke tahap deploy? (Click "Proceed" to continue)'
        }
    }
    stage('Deploy') { 
        docker.image('node:16-buster-slim').inside('-p 3000:3000') {
            sh './jenkins/scripts/deliver.sh'
            sleep(60)
            sh './jenkins/scripts/kill.sh'
        }
        sshagent(['production']) {
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12"
            sh "pwd"
            sh "rm -r /home/cloud/a428-cicd-labs"
            sh "git clone -b react-app https://github.com/arigints/a428-cicd-labs.git"
            sh "cd /home/cloud/a428-cicd-labs && docker build -t newreactimg ."
            sh "docker rm -f react-app && docker rmi react-app:latest -f && docker tag newreactimg:latest react-app:latest && docker rmi newreactimg:latest"
            sh "docker run --name react-app -d -p 3001:3000 react-app"
        }
    }
}