pipeline {
    agent any 
tools {nodejs "nodejs"}
    stages {
        stage('Build') { 
            steps {
                echo 'Building'
                sh 'git pull origin master'
                sh 'npm install'
                sh 'npm run build'
                
                emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'czeslave@gmail.com',
                subject: "Build result"
            }
        }
        stage('Test') { 
            when{
                expression {"SUCCESS".equals(currentBuild.currentResult)}
            }
            steps {
                echo 'Testing'
                sh 'npm run test'
            }
        }
        stage('Deploy') { 
            steps {
                echo 'Deploying'
            }
        }
    }

    post {
        failure {
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'czeslave@gmail.com',
                subject: "Build failed in Jenkins ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
        success {
            emailext attachLog: true,
                body: "${currentBuild.currentResult}: Job ${env.JOB_NAME} build ${env.BUILD_NUMBER}",
                recipientProviders: [developers(), requestor()],
                to: 'czeslave@gmail.com',
                subject: "Successful build in Jenkins ${currentBuild.currentResult}: Job ${env.JOB_NAME}"
        }
    }
}
