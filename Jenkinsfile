node {
    stage('Build') {
        sshagent(['production']) {
            sh "ssh -o StrictHostKeyChecking=no -l cloud 52.139.171.12"
            sh "pwd"
            sh "whoami"
            sh "ls"
            sh "docker ps"
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
            sh "whoami"
            sh "ls"
            sh "docker ps"
        }
    }
}