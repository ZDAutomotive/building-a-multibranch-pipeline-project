pipeline {
    agent {
        docker {
            image 'node:6-alpine' 
            args '-p 5000:5000' 
        }
    }
    environment {
        CI = 'true'
    }
    stages {
        stage('Build') { 
            steps {
                sh 'npm install' 
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
                sh 'echo "${isRebuild}"'
                sh '''
                    if [ ${isRebuild} ]; then
			        echo "is rebuild"
                    fi
                   '''
            }
        }
        // stage('Deliver') {
        //     steps {
        //         sh './jenkins/scripts/deliver-for-development.sh'
        //         input message: 'Finished using the web site? (Click "procees" to continue)'
        //         sh './jenkins/scripts/kill.sh'
        //     }
        // }
    }
    post {
        failure {
            mail bcc: 'chao.su+test1@zd-automotive.de', body: "<b>Example</b><br>\n <br>Project: ${env.JOB_NAME} <br>Build Number: ${env.BUILD_NUMBER} <br> URL de build: ${env.BUILD_URL}", cc: '', charset: 'UTF-8', from: '', mimeType: 'text/html', replyTo: '', subject: "ERROR CI: Project name -> ${env.JOB_NAME}", to: "chao.su+dev@zd-automotive.de";
        }
    }
}