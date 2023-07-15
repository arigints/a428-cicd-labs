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
            //input message: 'Lanjutkan ke tahap deploy? (Click "Proceed" to continue)'
        }
    }
    stage('Deliver') { 
        if (env.SERVER_ROLE == "PRODUCTION") {
            sh "rm -r /home/cloud/a428-cicd-labs"
            sh "git clone -b react-apps https://github.com/arigints/a428-cicd-labs.git"
            sh "cd /home/cloud/a428-cicd-labs/scripts && npm install"
            sh "sudo bash /home/cloud/a428-cicd-labs/jenkins/scripts/deliver.sh"
        } else {
            docker.image('node:16-buster-slim').inside('-p 3000:3000') {
                sh './jenkins/scripts/deliver.sh'
                sleep(60)
                sh './jenkins/scripts/kill.sh'
            }
            docker.image('curlimages/curl').inside{
                sh "curl -u ${env.JENKINS_PROD_USERNAME}:${env.JENKINS_PROD_PASSWORD} ${env.JENKINS_PROD_ENDPOINT}/job/react-apps/build?token=${env.JENKINS_PROD_TOKEN}"
            }
        }
    }
}