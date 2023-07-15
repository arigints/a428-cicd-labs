node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000'){
        stage('Build') {
            checkout scm
            sh 'npm install'
        }
        stage('Test') { 
            sh './jenkins/scripts/test.sh' 
            input message: 'Lanjutkan ke tahap deploy? (Click "Proceed" to continue)'
        }
        stage('Deliver') { 
            sh './jenkins/scripts/deliver.sh'
            sleep(60)
            sh './jenkins/scripts/kill.sh'
            docker.image('curlimages/curl').inside{
                sh "curl tokopedia.com"
            }
        }
    }
}
